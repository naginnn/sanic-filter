# Sanic filter
**Required:**

- Python: >=3.9
- SQLAlchemy: >=2.0.20

**Optional**

- Sanic: >=23.3.0
- Dataclasses

## Installation

```bash
pip install sanic-filter
```

## Description

You define the fields you want to be able to filter on as well as the type of operator
```bash
name: str = Query(option='ilike')
```
You can make some field mandatory
```bash
class MyFilter(Filter)
	name: str = Query(option='ilike', required=True)
```

Query
>?name=moon,sun

sqlAlchemy
>SELECT * FROM table WHERE table.name LIKE :name_1 OR table.name LIKE :name_2"

>Supported operators:
>- like
>- ilike
>- in
>- ==
>- !=

If you want to search by JSON fields

#SqlAlchemy model
```bash
properties = Column(JSON, nullable=True)
```
Filter supports one level of nesting
```bash
color: str = Query(option='ilike’, json_column='properties', required=True)
```
Query
>?name=moon,sun

sqlAlchemy
>SELECT * FROM table WHERE table.name LIKE :name_1 OR table.name LIKE :name_2"

# Returned fields

You can specify the returned fields of the models
Filter
```bash
fields: str = Query(option='returned_fields')
```

It will look like this
SqlAlchemy model
```bash
class Car(BaseModel):
    __tablename__ = "cars"

    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
    name = Column(String, nullable=True)
    order = Column(String, nullable=True)
    properties = Column(JSON, nullable=True)
```
Query
>?name=moon,sun

sqlAlchemy
>SELECT * FROM table WHERE table.name LIKE :name_1 OR table.name LIKE :name_2"


The search for fields is performed automatically, if the field has the same name, use model name

sqlalchemy models
```bash
class Car(BaseModel):
    __tablename__ = "cars"

    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)

class Garage(BaseModel):
    __tablename__ = «garages»

    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
```
filter
```bash
id: str = Query(option='ilike', required=True, model=Car)
```

routers

```bash
query.filter(select(Car, Garage))
```
If you want to return fields with the same name, but from different models, use the alias parameter
id from Car
```bash
car_id: str = Query(option='ilike’, alias=‘id’, model=Car)
```
```bash
garage_id: str = Query(option='ilike', alias=‘id’, model=Garage)
```