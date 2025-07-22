# Security

We provide here some tips based on our experience. 

!!! warning 
    We are by **no** mean security experts. Cybersecurity is a very active domain and unfortunately 
    suffers from a serious shortage of <a href=https://www.csoonline.com/article/657598/cybersecurity-workforce-shortage-reaches-4-million-despite-significant-recruitment-drive.html class="external-link" target="_blank">talent/expertise</a>.
    Do not hesitate to request audit from certified experts.

## Use multi-stage build 

Docker <a href=https://docs.docker.com/build/building/multi-stage/ class="external-link" target="_blank">Multi stage build</a> functionality has two main benefits: 

- Reduce the image size (sometimes up to a factor 10)
- Reduce the surface attack: only what is absolutely necessary is used at runtime, not what was used to build packages. 

Example of a simple multi-stage build script would be 
```yaml
###########
# BUILDER #
###########
FROM python:3.11-slim as builder

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# install packages
RUN python3 -m pip install --upgrade pip
COPY ./requirements.txt /requirements.txt
RUN python3 -m pip wheel --no-cache-dir --no-deps --wheel-dir /app/wheels -r requirements.txt


#########
# FINAL #
#########
FROM python:3.11-slim

# Install build packages
COPY --from=builder /app/wheels /wheels
RUN python3 -m pip install --no-cache /wheels/*

## Ship code
COPY app/ /app/app
WORKDIR /app

#Opened ports
EXPOSE 80

ENV PYTHONPATH=/app

CMD ["uvicorn", "app.dash_app:server", "--host", "0.0.0.0", "--port", "80", "--timeout-keep-alive", "300", "--ws-ping-timeout", "300" ]
```

The use of `PYTHONDONTWRITEBYTECODE` and `PYTHONUNBUFFERED` is well explained <a href=https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/ class="external-link" target="_blank">here</a> 
(very good site and a great source of inspiration for us)

## Use Open source image scanners 

We can recommend <a href=https://github.com/anchore/syft class="external-link" target="_blank">syft</a>  that works hand in hand with <a href=https://github.com/anchore/grype class="external-link" target="_blank">grype</a>.

After installation, using syft is as simple as 
```properties
syft <YOUR_DOCKER_IMAGE> -o cyclonedx-json=sbom.json
```

Go <a href=https://www.techtarget.com/whatis/definition/software-bill-of-materials-SBOM class="external-link" target="_blank">here</a> to learn what is a Software Bill Of Materials (SBOM).

Then in the folder containing your `sbom.json`, using grype (after installation) is as simple as:

```properties
grype sbom:sbom.json
```

## Rebuild regularly your docker images 

Most of the critical vulnerabilities of popular python packages/Ubuntu components are patched very fast. 

You should therefore regularly rebuild your docker image. 

!!! Important
    You should use the `no-cache`  <a href=https://docs.docker.com/engine/reference/commandline/image_build/ class="external-link" target="_blank">option</a>. 
    Otherwise a `latest` image won't be pulled for instance. 


For instance, on an image not built since some weeks, here is the `grype` report 

<p align="center">
  <a href="/img/ecodev_infra/grype_before.png"><img src="/img/ecodev_infra/grype_before.png" alt="Before rebuilding"></a>
</p>
<p align="center">
    <em>Before rebuilding</em>
</p>
<p align="center">
</p>

And for the same image, rebuilt with the `no-cache`

<p align="center">
  <a href="/img/ecodev_infra/grype_after.png"><img src="/img/ecodev_infra/grype_after.png" alt="After rebuilding"></a>
</p>
<p align="center">
    <em>After rebuilding</em>
</p>
<p align="center">
</p>


The resource that triggered our interest on the topic can be found  <a href=https://www.youtube.com/watch?v=1PBXB3RcDLs&t=1s&ab_channel=DreamsofCode class="external-link" target="_blank">here</a> 

## Use Open source code scanners 

There are numerous tools that allow you to directly scan your code base (by contrast with scanning docker images). 

This allow for a more precise analysis, very precious in an era where cyberattacks are all the more present. 

To at least inspect how you perform with respect to the <a href=https://owasp.org/www-project-top-ten/ class="external-link" target="_blank">OWASP Top Ten</a>,
you can use <a href=https://www.sonarsource.com/products/sonarqube/ class="external-link" target="_blank">SonarQube</a>.

It comes with Github integration, but you can even use it locally, following these steps. First run

```properties
docker run -d \ 
  --name sonarqube \
  -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true \
  -p 9001:9000 \
  sonarqube:latest
```

Then go to http://127.0.0.1:9001. After login (admin admin by default) you should create a project like so 


<p align="center">
  <a href="/img/ecodev_infra/sonar_1.png"><img src="/img/ecodev_infra/sonar_1.png" alt="Create project"></a>
</p>
<p align="center">
    <em>Create project</em>
</p>
<p align="center">
</p>

<p align="center">
  <a href="/img/ecodev_infra/sonar_2.png"><img src="/img/ecodev_infra/sonar_2.png" alt="Use global settings"></a>
</p>
<p align="center">
    <em>Use global settings</em>
</p>
<p align="center">
</p>

<p align="center">
  <a href="/img/ecodev_infra/sonar_3.png"><img src="/img/ecodev_infra/sonar_3.png" alt="Local project"></a>
</p>
<p align="center">
    <em>Local project</em>
</p>
<p align="center">
</p>

<p align="center">
  <a href="/img/ecodev_infra/sonar_4.png"><img src="/img/ecodev_infra/sonar_4.png" alt="Generate token"></a>
</p>
<p align="center">
    <em>Generate token</em>
</p>
<p align="center">
</p>

<p align="center">
  <a href="/img/ecodev_infra/sonar_5.png"><img src="/img/ecodev_infra/sonar_5.png" alt="Copy token"></a>
</p>
<p align="center">
    <em>Copy token</em>
</p>
<p align="center">
</p>

Once all of this is done, you can launch

```properties
sudo docker run \                                                                                                                                                                 ─╯
    --rm \
    -e SONAR_HOST_URL="http://127.0.0.1:9001" --network=host \
    -e SONAR_SCANNER_OPTS="-Dsonar.projectKey=YOUR_PROJECT_NAME" \
    -e SONAR_LOGIN="YOUR_API_KEY" \
    -v ./app:/usr/src \
    sonarsource/sonar-scanner-cli
```
setting `YOUR_PROJECT_NAME` and `YOUR_API_KEY` accordingly. This command should be launched in a code repo where the actual code is in `app` (look at the mounted volume).

Once the command has finished running, you can inspect in the web interface the results

<p align="center">
  <a href="/img/ecodev_infra/sonar_6.png"><img src="/img/ecodev_infra/sonar_6.png" alt="Inspect results"></a>
</p>
<p align="center">
    <em>Damn you funky police in typography.css. Only "bugs" for a python project...</em>
</p>
<p align="center">
</p>


## Use IP whitelisting

If all what is described [here](setup/setup_ubuntu_vm.md) does not suffice, you can add <a href=https://doc.traefik.io/traefik/middlewares/http/ipwhitelist/ class="external-link" target="_blank">IP white sourcing</a>: only listed IPs will be allowed 
to connect to your app.

Setting it up is quite simple thanks to Traefik. In the service label section of the docker-compose stack you want to protect, just add

```yaml
- traefik.http.middlewares.NAME.ipwhitelist.sourcerange=1.2.3.4/32, 5.6.7.8
- traefik.http.routers.SERVICE_NAME-https.middlewares=NAME
```

Where `NAME` is the name you want to give to the `ipwhitelist` middleware, and `SERVICE_NAME` the service defined in the docker-compose stack section you are 
currently editing.
 
!!! Subtlety
    if you create all of your stacks with our template, there is always a middleware to redirect http to https. 
    This is why we create the ipwhitelist middleware at the https level. Otherwise it would not be applied 😁