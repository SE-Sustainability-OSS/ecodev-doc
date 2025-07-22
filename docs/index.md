<p align="center">
  <a href="/img/cda.png"><img src="/img/cda.png" alt="EcoAct-dev"></a>
</p>
<p align="center">
    <em>Python/Docker open source ressources to help small scale structure create web applications</em>
</p>
<p align="center">
</p>

---
**Source Code ecodev-app**: <a href="https://github.com/SE-Sustainability-OSS/ecodev-app" target="_blank">https://github.com/SE-Sustainability-OSS/ecodev-app</a>

**Documentation**: <a href="https://ecodev-doc.lcabox.com/" target="_blank">[https://ecoact-dev.lcabox.fr](https://ecodev-doc.lcabox.com/)/</a>

**Source Code ecodev-infra**: <a href="https://github.com/SE-Sustainability-OSS/ecodev-infra" target="_blank">https://github.com/SE-Sustainability-OSS/ecodev-infra</a>

**Source Code ecodev-core**: <a href="https://github.com/SE-Sustainability-OSS/ecodev-core" target="_blank">https://github.com/SE-Sustainability-OSS/ecodev-core</a>

**Source Code ecodev-cloud**: <a href="https://github.com/SE-Sustainability-OSS/ecodev-cloud" target="_blank">https://github.com/SE-Sustainability-OSS/ecodev-cloud</a>

**Source Code ecodev-front**: <a href="https://github.com/SE-Sustainability-OSS/ecodev-front" target="_blank">https://github.com/SE-Sustainability-OSS/ecodev-front</a>

---

## Why this?

**Ecoact CDA team** has been developping more than 30 python apps since its creation at the beginning of 2023. It was able to do so thanks to four fantastic python libraries:

- <a href=https://pydantic-docs.helpmanual.io/ class="external-link" target="_blank">Pydantic</a>: a way to morally transform python into a strongly typed language, easing code readibility and maintainability tremendously. Thanks to  <a href=https://github.com/samuelcolvin class="external-link" target="_blank">Samuel Covin</a> for this python jewel! 

- <a href=https://fastapi.tiangolo.com/ class="external-link" target="_blank">FastAPI</a>: A modern python app built on top of Pydantic that makes the api development a breeze. <a href=https://github.com/tiangolo class="external-link" target="_blank">Sebastien Ramirez (Tiangolo)</a> is a god, and there would be no python at EcoAct if not for him!!! We are forever in his debt.

- <a href=https://sqlmodel.tiangolo.com/ class="external-link" target="_blank">SQLModel</a>: Tiangolo did not stop at FastAPI, he also built an <a href=https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping class="external-link" target="_blank">ORM</a> on top of pydantic and <a href=https://www.sqlalchemy.org/ class="external-link" target="_blank">SQLAlchemy</a>, leaving us with the best of both words!

- <a href=https://dash.plotly.com/ class="external-link" target="_blank">Dash</a>: <a href=https://github.com/chriddyp class="external-link" target="_blank"> Chris Parmer</a>
 did python afficionados a favor: build a python library on top of react components, to allow for quick front developments fully in python. While you won't build an e-commerce website with Dash (although you never know...), it is a the perfect tool for us to build internal web apps in weeks instead of months, avoiding the unecessary multiplication of microservices.


While 95% of the generic cross-app code at **CDA** comes from these libraries (and the surrounding library ecosystem, as always very rich in python), we found that we had to repeat some boilerplate code in every one of our apps. We would thus like to give back to the community this factorization, hoping it will speed others to reach the same state as us. That way we will save precious ressources (both human and computing power)! We would of course prefer that apps developed with **Ecodev** libraries/repos do not directly compete with ours, but we are willing to take this risk by releasing these codes public because we truly think it can have a small reduction impact in terms of IT consumption ressources.



## What is this

In a nutshell:

<a href="/img/ecodev.png"><img src="/img/ecodev.png" alt="EcoAct-dev"></a>

**Ecodev** is the regroupment of 

- 3  <a href=https://pypi.org/ class="external-link" target="_blank">python libraries</a> 
- 2 <a href=https://github.com/cookiecutter/cookiecutter/tree/5d2b1e37b90ad11ac412c691f131689445840709/ class="external-link" target="_blank">cookicutter templates</a>
- 1 <a href=https://ecoact-dev.lcabox.fr/  target="_blank">documentation</a> (you are currently reading it 😊)

It serves several usecases, depending on your level of maturity:

- You start from a Blank slate (well  <a href=https://en.wikipedia.org/wiki/The_Blank_Slate class="external-link" target="_blank">it does not exist</a> 😋): follow the w̶h̶i̶t̶e̶ ̶r̶a̶b̶b̶i̶t̶ documentation [from scratch](setup-from-scratch/index.md)
- You read the setup [from scratch](setup-from-scratch/index.md) (and actually did the setup 😋) and want to see what you can quickly get out of it: go to [ecodev-app](cookiecutters/app/index.md) documentation 
- You want to understand the tiny details related to:

    - the backend: go to the [ecodev-core](libraries/core/index.md) documentation
    - the frontend: go to the [ecodev-front](libraries/front/index.md) documentation
    - the infra: go to the [ecodev-infra](cookiecutters/infra/index.md) documentation
  
- You want to easily read/write from S3/Blob storages, go to the [ecodev-cloud](libraries/cloud/index.md) documentation

## License

This project is licensed under the terms of the [MIT license](https://github.com/SE-Sustainability-OSS/ecodev-app/blob/main/LICENSE).
