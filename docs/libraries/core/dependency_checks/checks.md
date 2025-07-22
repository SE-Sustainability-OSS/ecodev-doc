# Dependency checks 

In our different web apps, we typically have a structure of the form

<p align="center">
  <a href="/img/ecodev_core/dependencies.png"><img src="/img/ecodev_core/dependencies.png" alt="App dependencies"></a>
</p>
<p align="center">
    <em>Typical code base high level dependencies</em>
</p>
<p align="center">
</p>

Where each blue rectangle corresponds to a regroupment of several python modules (for complex apps, could be on the order 10-100).

An oriented arrow between A and B meaning that B depends on some modules in A, but **not** the other way around. 

For instance, we would like our domain model classes and enums not to depend on the web app pages. 

To enforce this, we use <a href=https://pre-commit.com/ class="external-link" target="_blank">pre-commit</a> hook that "freezes" these high level dependencies. 

## `check_dependencies`

We thus typically put in our `.pre-commit-config.yaml` file for a repo handling `CONTAINER` the following part


```yaml
-   repo: local
    hooks:
    -   id: check_dependencies
        name: check_dependencies
        entry: docker exec CONTAINER python3.11 -c 
               "from ecodev_core import check_dependencies;
                exit(not check_dependencies(code_base_path,  expected_deps_file))"
        language: system
```

where 

- `code_base_path` is the path to where the actual code of the code base sits (inside `CONTAINER`) 
- `expected_deps_file` is the path towards the expected (frozen, chosen in advance) dependencies.

 This is a text file that takes the form
  ```txt
  module x depends on,core,db,insertors,nlp,pages,retrievers,routers
  core,Yes,No,No,No,No,No,No
  db,Yes,Yes,No,No,No,No,No
  insertors,Yes,Yes,Yes,No,No,No,No
  nlp,Yes,No,No,Yes,No,No,No
  pages,Yes,Yes,No,No,No,No,No
  retrievers,Yes,Yes,No,Yes,No,Yes,No
  routers,Yes,Yes,Yes,No,No,Yes,Yes
  ```
  so a dependency matrix between the regroupment of modules of interest. 

`check_dependencies` is built on top of the awesome <a href=https://github.com/thebjorn/pydeps class="external-link" target="_blank">pydeps</a>. 

When running on a "working" code base (that has not unintentionally modified high level dependencies), the pre-commit hook returns like so: 

<p align="center">
  <a href="/img/ecodev_core/okcheck.png"><img src="/img/ecodev_core/okcheck.png" alt="OK check"></a>
</p>
<p align="center">
    <em>OK pre-commit hook returns</em>
</p>
<p align="center">
</p>

And in case of a change, like so:

<p align="center">
  <a href="/img/ecodev_core/kocheck.png"><img src="/img/ecodev_core/kocheck.png" alt="KO check"></a>
</p>
<p align="center">
    <em>KO pre-commit hook returns</em>
</p>
<p align="center">
</p>

To learn more on `check_dependencies`, you can go inspect the  <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/unitary/test_dependencies.py class="external-link" target="_blank">test suite</a>


## `compute_dependencies`

To automatically compute dependencies and produce the txt file snippet of [the previous part](#check_dependencies),
you can call the `compute_dependencies` method 

```python
compute_dependencies(code_base_path: Path, output_folder: Path, plot: bool = True)
```

where 

- `code_base_path` is the path to where the actual code of the code base sits
- `output_folder` is where the output text file should be written
- `plot` is a boolean indicating whether `pydeps` should generate a png output plot or not 

Plot looks like so

<p align="center">
  <a href="/img/ecodev_core/deps.png"><img src="/img/ecodev_core/deps.png" alt="Imaginary dependencies"></a>
</p>
<p align="center">
    <em>An example of a dependency graph at the regroupment of modules level produced by pydeps</em>
</p>
<p align="center">
</p>

As explained in the <a href=https://github.com/thebjorn/pydeps class="external-link" target="_blank">pydeps</a> documentation, you will need to install 
<a href=https://www.graphviz.org/download/ class="external-link" target="_blank">graphviz</a>

To learn more on `compute_dependencies`, you can go inspect the  <a href=https://github.com/SE-Sustainability-OSS/ecodev-core/blob/main/tests/unitary/test_dependencies.py class="external-link" target="_blank">test suite</a>

If you want to learn about other code good practices that you can enforce, we recommend these (biased 😊) readings/videos

- <a href=https://google.github.io/styleguide/pyguide.html class="external-link" target="_blank">Google Python Style Guide</a>
- <a href=https://www.youtube.com/@ArjanCodes class="external-link" target="_blank">ArjanCodes</a>, very high quality ressources on various python related topics. 
- <a href=https://neptune.ai/blog/building-ml-pipeline-problems-solutions class="external-link" target="_blank">Google Blog post written by one of us</a>, Solution 6.
If you're doing ML, the rest of the post might prove useful (and <a href=https://neptune.ai/ class="external-link" target="_blank">neptune.ai</a> too! 😊). 
