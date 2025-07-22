# Reports

<p align="center">
  <a href="/img/ecodev_app/report.png"><img src="/img/ecodev_app/report.png" alt="Reports"></a>
</p>
<p align="center">
    <em>Reports</em>
</p>
<p align="center">
</p>

Simple page illustrating how to create a report.
 
Code is awfully simple and makes use under the hood of [report_value](../../../libraries/front/components/report_value.md)

```python
def get_example_report() -> dmc.Card:
    """
    Components for a report example
    """
    return background_card(
        [
            dmc.Stack(
                [
                    card_title('Example output report', align='center'),
                    dmc.SimpleGrid(
                        cols=2,
                        children=[
                            report_value('Scope 1', 15_000, 'kgCO2e'),
                            report_value('Scope 2', 32_000, 'kgCO2e'),
                        ],
                    ),
                    dmc.Space(h=5),
                    dmc.Divider(),
                    dmc.Space(h=5),
                    dmc.SimpleGrid(
                        cols=3,
                        children=[
                            report_value('Scope 3 - Cat 1', 15_000_000, 'kgCO2e'),
                            report_value('Scope 3 - Cat 2', 15_000_000_000, 'kgCO2e'),
                            report_value('Scope 3 - Cat 3', 15_000, 'kgCO2e'),
                            report_value('Scope 3 - Cat 4', 15_000, 'kgCO2e'),
                            report_value('Scope 3 - Cat 5', 15_000, 'kgCO2e'),
                            report_value('Scope 3 - Cat 6', 15_000, 'kgCO2e'),
                            report_value('Scope 3 - Cat 7', 15_000, 'kgCO2e'),
                            report_value('Scope 3 - Cat 8', 15_000, 'kgCO2e'),
                        ],
                    ),
                ]
            )
        ]
    )
```