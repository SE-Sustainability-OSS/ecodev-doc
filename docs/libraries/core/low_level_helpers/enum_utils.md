# Enum helpers

Helper methods to convert stuff to enums. Right now there is just `enum_converter`

## `enum_converter`

Convert a passed `str` to a passed `Enum` type, if possible. returns `None` otherwise. 

If unfamiliar, learn more about enums <a href=https://docs.python.org/3/library/enum.html class="external-link" target="_blank">here</a>. TL DR: you should definitely use them! 😊

```python 
self.assertEqual(enum_converter('Admin', Permission), Permission.ADMIN)
self.assertTrue(enum_converter('toto', Permission) is None)
```

See `Permission` [here](../authentication/app_user.md/#app-user).