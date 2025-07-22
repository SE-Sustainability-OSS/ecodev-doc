# Pydantic helpers

Opinionated pydantic base classes that we use on a daily basis 

## `Frozen`

Everything in python (with rare exceptions like `str` and `tuples`) is mutable. 

This is convenient for prototyping: you tweak as need be and in no time every instance of every classes you create.

For production though, this is completely unacceptable. 
If we define a domain model class `foo` in a certain way, we want to be sure that several hundred lines of code later `foo` is still the same. 

We have seen certain ML code bases that were a nightmare to navigate through: all hyperparameters defined as a 500 line nested dictionary, and mutated all over the place.
Things become completely intractable at this point, executions are bloated with bugs, and precious time is lost on wild goose chases. 

For this reason, 99% of "domain model classes" (we use this expression quite a lot in this documentation, having been inspired as so many by Eric Evans Masterpiece: <a href=https://en.wikipedia.org/wiki/Domain-driven_design class="external-link" target="_blank">Domain Driven Design</a> )
we use at EcoAct inherits from our `Frozen` class:


```python
from pydantic import BaseModel
from pydantic import ConfigDict

class Frozen(BaseModel):
    """
    Frozen pydantic configuration
    """
    model_config = ConfigDict(frozen=True)
```

In the 1% of cases where domain classes are not frozen, it corresponds to case where iteratively building the class is part of its very nature, and forcing immutability would 
result in a lot of boilerplate code. We are usually adamant in the docstring of the class why we did not use `Frozen`, and encourage if possible future simple implementations that
would restore it.

## `Basic`

A simple wrapper around `BaseModel` that allows to have arbitrary types. We rarely use it, but when all else fails...

```python
from pydantic import BaseModel
from pydantic import ConfigDict

class Basic(BaseModel):
    """
    Basic pydantic configuration
    """
    model_config = ConfigDict(frozen=False, arbitrary_types_allowed=True)
```

## `CustomFrozen`

The moral combination of  [Frozen](#frozen) and [Basic](#basic) behaviours

```python
from pydantic import ConfigDict

class CustomFrozen(Frozen):
    """
    Frozen pydantic configuration for custom types
    """
    model_config = ConfigDict(arbitrary_types_allowed=True)
```

## `OrmFrozen`

We enquired on StackOverflow on to how to enforce immutability in SQLModel - as is done for Pydantic.

Turns out this apparently cannot be done <a href=https://stackoverflow.com/questions/74732161/sqlmodel-forbid-mutation class="external-link" target="_blank">as of today</a>.

But stay tune, as soon as it is feasible we will have a working `OrmFrozen`! 😊