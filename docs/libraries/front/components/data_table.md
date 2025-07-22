# Dash ag grid table

Opinionated wrapper around the <a href=https://dash.plotly.com/dash-ag-grid class="external-link" target="_blank">Dash Ag Grid</a> component.

To use the <a href=https://dash.plotly.com/dash-ag-grid/enterprise-ag-grid class="external-link" target="_blank">Enterprise</a> features, provide the 
following environment variables: 

```python
class DashAgGridEnterprise(BaseSettings):
    """
    Simple authentication configuration class
    """
    dag_license_key: str = ''
    enable_dag_enterprise: bool = False
    model_config = SettingsConfigDict(env_file='.env')
```

Stay tuned for more information on server side filtering 😊.

Rendering:

<p align="center">
  <a href="/img/ecodev_front/dag.png"><img src="/img/ecodev_front/dag.png" alt="Dash Ag Grid"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>