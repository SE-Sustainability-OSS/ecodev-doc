# Safe helpers

Low level helpers related to safe code execution

## `SafeTestCase`

In recent years, two test frameworks see the most use 
- <a href=https://docs.python.org/3/library/unittest.html class="external-link" target="_blank">unittest</a>: the ancestor 😋
- <a href=https://docs.pytest.org/en/7.4.x/ class="external-link" target="_blank">pytest</a>: the wannabe replacer 😋

For starting a new project, people usually advocate to use pytest. 

But we start python a long time ago and went with unittest at the time. It never failed us, and this is why unnitest is used at EcoAct.

One recurring complain about unittest is the additionnal boilerplate code you need to make it work as pytest. This is true up to a certain extent true, but we 
developed `SafeTestCase` to centralized this boilerplate code. By replacing unnitest `TestCase` with our `SafeTestCase`, we hope to avoid you the boilerplate. 

`SafeTestCase` has two important attributes:

- `directories_created`: a list to which you can append directories meant to be used in one test and erased at the end of it.
- `files_created`:  a list to which you can append files meant to be used in one test and erased at the end of it.


In addition, you get out of the box these kind of logs (we plug on our [logger](../logging/logger.md))

<p align="center">
  <a href="https://ecoact-dev.lcabox.fr/"><img src="/img/ecodev_core/tests_run.png" alt="tests run"></a>
</p>
<p align="center">
    <em>Logs embedded with SafeTestCase</em>
</p>
<p align="center">
</p>

We hope you'll find `SafeTestCase` as convenient as it is for us 😊.
 
## `SimpleReturn`

Usually the return of all our POST routes, inspired from the <a href=https://en.wikibooks.org/wiki/Haskell/Understanding_monads/Maybe class="external-link" target="_blank">maybe monad</a>.

Standard pattern in C#, and especially in rust. You could alo find it in python in the <a href=https://github.com/rustedpy/result class="external-link" target="_blank">result library</a>. 

Its code is quite simple and hopefully enlightening.

```python
from ecodev_core.pydantic_utils import Frozen
from pydantic import Field
from typing import Union

class SimpleReturn(Frozen):
    """
    Simple output for routes not returning anything
    """
    success: bool = Field(..., description=' True if the treatment went well.')
    error: Union[str, None] = Field(..., description='the error that happened, if any.')

    @classmethod
    def route_success(cls) -> 'SimpleReturn':
        """
        Format DropDocumentReturn if the document was successfully dropped
        """
        return SimpleReturn(success=True, error=None)

    @classmethod
    def route_failure(cls, error: str) -> 'SimpleReturn':
        """
        Format DropDocumentReturn if the document failed to be dropped
        """
        return SimpleReturn(success=False, error=error)
```

## `stringify`

Safe conversion of a (str, np.nan) value into a (str,None) one

```python
# test stringify behaviour
self.assertEqual(stringify(3), '3')
self.assertEqual(stringify(np.nan), None)
self.assertEqual(stringify('toto'), 'toto')
self.assertEqual(stringify('3'), '3')
```

## `boolify`

Safe conversion of a (str, np.nan) value into a (bool,None) one. By convention, Yes/yes/No/no are mapped to `True/True/False/False`.

```python
# test boolify behaviour
self.assertEqual(boolify('true'), True)
self.assertEqual(boolify('yes'), True)
self.assertEqual(boolify('True'), True)
self.assertEqual(boolify('Yes'), True)
self.assertEqual(boolify('false'), False)
self.assertEqual(boolify('no'), False)
self.assertEqual(boolify('False'), False)
self.assertEqual(boolify('No'), False)
self.assertEqual(boolify(np.nan), None)
self.assertEqual(boolify(True), True)
self.assertEqual(boolify(False), False)
self.assertEqual(boolify('toto'), None)
self.assertEqual(boolify(3), None)
self.assertEqual(boolify(None), None)
```


## `intify`

Safe conversion of a (int, np.nan) value into a (int,None) one

```python
# test intify behaviour
self.assertEqual(intify(3), 3)
self.assertEqual(intify(np.nan), None)
self.assertEqual(intify('toto'), None)
self.assertEqual(intify('3'), 3)
```


## `floatify`

Safe conversion of a (float, np.nan) value into a (float,None) one

```python
# test floatify behaviour
self.assertEqual(floatify(3), 3.0)
self.assertEqual(floatify(np.nan), None)
self.assertEqual(floatify('toto'), None)
self.assertEqual(floatify('3.0'), 3.0)
```

## `safe_clt`

A method wrapper to return a `SimpleReturn` corresponding to success/failure execution of that said method. 


Simple illustration

```python
@safe_clt
def safe_divide(a: int, b: int):
    """
    Safe test divide for testing purposes.
    """
    return a / b


# Test that safe wrapper is working as intended.
self.assertEqual(safe_divide(1, 2), SimpleReturn(success=True, error=None))
self.assertEqual(safe_divide(1, 0), SimpleReturn(success=False, error='division by zero'))
```