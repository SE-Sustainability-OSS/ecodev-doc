# Pre-commit hooks

You can find the <a href=https://pre-commit.com/ class="external-link" target="_blank">pre-commit</a> hook file  
<a href=https://github.com/SE-Sustainability-OSS/ecodev-app/blob/main/.pre-commit-config.yaml class="external-link" target="_blank">here</a>.

Aside from the choice of not using <a href=https://github.com/astral-sh/ruff class="external-link" target="_blank">ruff</a> (Default configuration not adapted to our taste),
the last hook is a home made one ("compliment de la maison" 😂)

```yaml
-   repo: local
    hooks:
    -   id: check_dependencies
        name: check_dependencies
        entry: docker exec ecodev_app python3.11 -c "from ecodev_core import check_dependencies; from pathlib import Path;
            exit(not check_dependencies(Path('/app/app'), Path('/app/app/assets/dependencies.txt')))"
        language: system
```

it makes use of [ecodev-core](../../../libraries/core/dependency_checks/checks.md) helper methods to ensure that no 
circular references are created at the regroupment of module level (you do not want your domain model to depend on your dash pages for instance 😅).

To edit the authorized dependencies as you see fit, 
just edit this <a href=https://github.com/SE-Sustainability-OSS/ecodev-app/blob/main/app/assets/dependencies.txt class="external-link" target="_blank">configuration file</a>.

<p align="center">
  <a href="/img/ecodev_app/code.png"><img src="/img/ecodev_app/code.png" alt="Dependency example"></a>
</p>
<p align="center">
    <em>The out of the box dependencies enforced in ecodev-app</em>
</p>
<p align="center">
</p>