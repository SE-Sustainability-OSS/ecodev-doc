# Login component

Defining a simple login component: 

- a username  <a href=https://www.dash-mantine-components.com/components/textinput class="external-link" target="_blank">dmc TextInput</a>
- a <a href=https://www.dash-mantine-components.com/components/passwordinput class="external-link" target="_blank">dmc PasswordInput</a>  
- a <a href=https://www.dash-mantine-components.com/components/button class="external-link" target="_blank">dmc Button</a>.

All attributes are optional:

- the `username_placeholder` (`Username` by default)
- the `password_placeholder` (`Password` by default)
- the `button_label` (`Login` by default)
- the `button_color` (`white` by default)
          password_placeholder: str = 'Password',
          button_label: str = 'Login',
          button_color: str = 'white'

  
Rendering:

<p align="center">
  <a href="/img/ecodev_front/login.png"><img src="/img/ecodev_front/login.png" alt="Login components"></a>
</p>
<p align="center">
    <em>An example of such a component in a dash application</em>
</p>
<p align="center">
</p>
