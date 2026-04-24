---
created: 2026-01-27
last_edited: 2026-04-24
tags:
  - python
  - sql
  - sqlalchemy
  - orm
  - psycopg2
  - postgresql
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[02 Dimensional Modeling Kimball]]"
  - "[[06 DRY and Macros]]"
  - "[[PySpark Concepts]]"
ai_generated: true
human_approved: false
category:
  - Knowledge Base
  - Data
---
## psycopg2 vs SQLAlchemy

| Aspect            | psycopg2                | SQLAlchemy                            |
| ----------------- | ----------------------- | ------------------------------------- |
| Type              | Database adapter/driver | SQL toolkit + ORM                     |
| Abstraction Level | Low-level               | High-level                            |
| Control           | Full SQL control        | ORM abstraction with optional raw SQL |

An ORM (Object-Relational Mapping) library is a programming tool that creates a "bridge" between object-oriented programming languages and relational databases. It allows developers to work with database data using objects and methods instead of writing raw SQL queries.

### Without ORM (Traditional Approach)

```
import psycopg2

# Direct SQL - managing connections, cursors manually
conn = psycopg2.connect("dbname=test user=postgres")
cur = conn.cursor()
cur.execute("SELECT * FROM users WHERE id = %s", (1,))
user_data = cur.fetchone()
```

### With ORM

```
from sqlalchemy.orm import sessionmaker
from models import User

# Working with Python objects
session = SessionLocal()
user = session.query(User).filter(User.id == 1).first()
print(user.name)  # Direct attribute access
```
