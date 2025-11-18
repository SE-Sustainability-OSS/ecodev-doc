## Rest API Configuration

Configuration for making API calls to other apps using this stack. The environment variables can be defined either in an .env file or secrets/prod.yaml or secrets/local.yaml **placed in the root of the application**.

### .env file setup
```bash
host=https://example.com
user=test_user
password=test_password
```

### yaml file setup
Copy the [config/local.yaml](https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/config/local.yaml) file into [secrets/prod.yaml]() and  [secrets/local.yaml]() and fill the environment variables defined under the api section.

```yaml
api:
    host: https://example.com
    user: test_user
    password: test_password
```