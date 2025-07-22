# Form

<p align="center">
  <a href="/img/ecodev_app/form.png"><img src="/img/ecodev_app/form.png" alt="Form"></a>
</p>
<p align="center">
    <em>Form</em>
</p>
<p align="center">
</p>

A simple page illustrating how to create a form component. 

The code is quite self-explanatory: 

```python
def get_example_form() ->  dmc.Card:
    """
    Components for a form example
    """
    return background_card(
        [
            dmc.Stack(
                [
                    card_title('Example input form', align='left'),
                    dmc.SimpleGrid(
                        cols=3,
                        children=[
                            dmc.TextInput(label='Input text'),
                            dmc.Select(label='Input select'),
                            dmc.MultiSelect(label='Input multi-select'),
                        ],
                        style={'text-align': 'left'},
                    ),
                    dmc.Button('Submit', color='ecoact'),
                ]
            )
        ]
    )

```