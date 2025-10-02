# DB Internationalization

Internationalize SQLModel string attributes with the `I18nMixin` mixin. The mixin lets you declare localized variants of a string field (for example `name_en`, `name_fr`) and resolves the appropriate locale at runtime.

## Register localized fields

Define localized attributes and register them in `__localized_fields__`. The fallback language must always be available. Use the `Lang` enum to define the available languages at the model level.

## Example model

```python
from typing import Optional

from sqlmodel import SQLModel
from sqlmodel import Field

from ecodev_core import I18nMixin
from ecodev_core import Lang


class LocalizedModel(I18nMixin, SQLModel, table=True):  # type: ignore[misc]
    """Localized entity storing translated names."""

    __tablename__ = 'localized_model'
    __localized_fields__ = {'name': [Lang.EN, Lang.FR]}
    __fallback_lang__ = Lang.EN

    id: Optional[int] = Field(default=None, primary_key=True)
    name_en: str = Field(description='English display name')
    name_fr: Optional[str] = Field(default=None, description='French display name')

```

Here the field `name` is declared as localized with English and French variants. The fallback language (`Lang.EN`) is required (non nullable).

## Runtime locale resolution

```python
from sqlmodel import Session, select

from ecodev_core import engine
from ecodev_core import set_lang
from ecodev_core import Lang
from ecodev_core import localized_col

with Session(engine) as session:
    english_only = LocalizedModel(name_en='Hello')
    translated = LocalizedModel(name_en='Hello', name_fr='Bonjour')

    session.add(english_only)
    session.add(translated)
    session.commit()

    set_lang(Lang.FR)
    print(translated.name)  # Bonjour

    set_lang(Lang.EN)
    print(translated.name)  # Hello

    set_lang(Lang.EN)
    print(english_only.name)  # Hello

    set_lang(Lang.FR)
    print(english_only.name)  # Hello
```

`set_lang` uses a [`contextvars` context variable](https://docs.python.org/3/library/contextvars.html) so the active language flows through any functions executed within the same request/task.

```python
import contextvars

DB_LANG = 'db_lang'
CONTEXT_DB_LANG = contextvars.ContextVar(DB_LANG, default=Lang.EN)


def set_lang(lang: Lang) -> None:
    CONTEXT_DB_LANG.set(lang)


def get_lang() -> Lang:
    return Lang(CONTEXT_DB_LANG.get())
```

## Querying localized data

`localized_col` helps you build queries that use the active language (or an explicit override):

```python
set_lang(Lang.FR)
with Session(engine) as session:
    localized_query = select(
        localized_col('name', LocalizedModel),
        LocalizedModel.id,
    )
    print(session.exec(localized_query).all())

    localized_query = select(
        localized_col('name', LocalizedModel, Lang.EN),
        LocalizedModel.id,
    )
    print(session.exec(localized_query).all())

    filtered = select(LocalizedModel).where(
        localized_col('name', LocalizedModel) == 'Bonjour'
    )
    print(session.exec(filtered).all())
```

The first query yields localized values in French as defined using `set_lang` while the second yields localized values in english. The last query filters on the localized value. See the `db_i18n` functional tests in `ecodev-core` for more usage examples.
