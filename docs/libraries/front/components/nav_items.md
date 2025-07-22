# Nav Item components


Simple helpers around <a href=https://www.dash-mantine-components.com/components/menu class="external-link" target="_blank">dmc menu related</a>
components.

## Action item

 A navbar action item (i.e. icon button without a menu). 
 These can be opened in a new tab & can have callbacks associated with the id provided.

Mandatory Attributes are:

- `id`: the action item component id
- `label`:  the action item label
- `icon`: the action item icon
- `href`: the url reference (either internal or external) on which to redirect on click. 

Rendering:

<p align="center">
  <a href="/img/ecodev_front/action_item.png"><img src="/img/ecodev_front/action_item.png" alt="Action item"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>

## Menu item

Component to create a navbar menu item.

Mandatory Attributes are:

- `label`:  the action item label
- `icon`: the action item icon
- `href`: the url reference (either internal or external) on which to redirect on click. 

<p align="center">
  <a href="/img/ecodev_front/menu_item.png"><img src="/img/ecodev_front/menu_item.png" alt="Menu item"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>

## Menu 

 Component to create a navbar menu.
    The menu items must be created beforehand & passed to this component.

Mandatory Attributes are:

- `label`:  the action item label
- `icon`: the action item icon
- `menu_items`: the menu items (as defined in the previous section)

<p align="center">
  <a href="/img/ecodev_front/menu.png"><img src="/img/ecodev_front/menu.png" alt="Menu"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>

