[![Build Status](https://travis-ci.org/absent1706/sqlalchemy-mixins.svg?branch=master)](https://travis-ci.org/absent1706/sqlalchemy-mixins)
[![PyPI version](https://img.shields.io/pypi/v/sqlalchemy_mixins.svg)](https://pypi.python.org/pypi/sqlalchemy_mixins)
[![Python versions](https://img.shields.io/pypi/pyversions/sqlalchemy_mixins.svg)](https://travis-ci.org/absent1706/sqlalchemy-mixins)

A fork of sqlalchemy-mixins for starlette, especially fastapi.

## Features added by scil

- API for fastapi
  - `create_from_schema`
  - `init_from_schema`
- paginate. Powered by [exhuma/sqlalchemy-paginator@integration](https://github.com/exhuma/sqlalchemy-paginator/tree/integration). I am considerring [ djrobstep/sqlakeyset](https://github.com/djrobstep/sqlakeyset)  which is updated recently.

## Example

``` 
# base_class.py

from sqlalchemy_mixins import AllFeaturesMixin as SimpleBase  # noqa

class Base(SimpleBase):
    __abstract__ = True

    # Default auto incrementing primary key field
    # overwrite as needed
    id = sa.Column(sa.Integer, primary_key=True)

    @declared_attr
    def __tablename__(cls) -> str:
        return cls.__name__.lower()

    def __str__(self):
        return self.__repr__()

```


```
# app/models/item.py

from app.db import Base

class Item(Base):
    title = Column(sa.String, index=True)
    description = Column(sa.String, index=True)
    owner_id = Column(sa.Integer, ForeignKey("user.id"))
    owner = relationship("User", back_populates="items")

```

use
```
models.Item.create_from_schema(db, item_in, {'owner_id':current_user.id})
```

# About accent-starlette/starlette-core

Old version v2 based on [accent-starlette/starlette-core](https://github.com/accent-starlette/starlette-core)

Main unique feature: use customize session instead of session provided by starlette_core.
```
user = User()
user.save() # use starlette_core session
user.db(my_db)
user.save() # use my_db
user.db(None)
user.save() # use starlette_core session

User.create_from_schema(user_in, my_db) # use my_db

```

# SQLAlchemy mixins

see full docs in origian [absent1706/sqlalchemy-mixins](https://github.com/absent1706/sqlalchemy-mixins)