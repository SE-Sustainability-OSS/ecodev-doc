# App Layout

Simple helper around <a href=https://www.dash-mantine-components.com/components/appshell class="external-link" target="_blank">dmc MantineProvider</a>
component

##  Dash base layout 

All apps using Dash-mantine _must_ start from here to initialise Dash layout with Dash-Mantine.
It also include the a top navbar and a bottom footer.

The only mandatory attribute is 

  - `stores`: A list of tuple (store_name (used for the store id) /storage_type), 
     used to initialize corresponding <a href=https://dash.plotly.com/dash-core-components/store class="external-link" target="_blank">dcc Store</a>.
     The Stores are placed in the root of the app, to ensure that they are accessible. 


Optional attributes are: 

  - `main_color`: The main color used for the app
  - `colors`: A dictionary of color pallet name (key) and <a href=https://www.dash-mantine-components.com/components/mantineprovider#custom-colors class="external-link" target="_blank">dmc list of color values</a> 
  - `header_height`: the header height (default to 55)
  - `footer_height`: the footer height (default to 40)

Example of a dash app layout initialization

```python
ecoact_colors = {'ecoact': ['#DDF5FF',
                            '#81DBFF',
                            '#34C6FF',
                            '#00B3FF',
                            '#009CFF',
                            '#0082DE',
                            '#0066A1',
                            '#005794',
                            '#004576',
                            '#00385F']}


dash_app.layout = dash_base_layout(dash_stores, colors=ecoact_colors)
```