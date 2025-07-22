# Pie chart components


Simple helpers around <a href=https://plotly.github.io/plotly.py-docs/generated/plotly.graph_objects.Pie.html class="external-link" target="_blank">go Pie</a>
component.

## Pie chart

Helper method to create a pie chart.

Attributes are:

 - `labels`: column on which we want to perform an aggregation
 - `values`: numerical values that will be aggregated
 - `customdata`: list of str that represent a text we want to use in the hovertemplate
 - `hovertemplate`: str that must be formatted like html, that will show when you hover on the
 graph
 - `colors`: list of custom colors to be applied to the pie chart. Plotly default colors will
 be applied if not filled in
 - `text_threshold`: float between 0 and 1, that controls the minimum threshold in terms of
 percentage of the total under which category labels will be shown. The higher the threshold,
 the less categories will be shown (only the biggest one),
 - `annotation_text`: text that will be displayed at the center of the pie chart

Rendering:

<p align="center">
  <a href="/img/ecodev_front/pie_chart.png"><img src="/img/ecodev_front/pie_chart.png" alt="Pie chart"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>
