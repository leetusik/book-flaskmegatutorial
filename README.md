[Flask Mega-Tutorial](https://www.amazon.com/New-Improved-Flask-Mega-Tutorial-ebook/dp/B079KPG4HT/ref=sr_1_1?crid=3IVREYVPEUX2D&dib=eyJ2IjoiMSJ9.0mhRtTd6i2aJn4Sesierj5QSD8Oa9uAc1LE8XUO5D4pAVliY47tCLanADC03b5z0BZztHqam37u8hIxHe51_nHRSNefw6n93tzH-cyeF7S4.fbP0RoG7gR57KvYTPeP9Zmc7UIGoGZ4uoi0O4KvsBj0&dib_tag=se&keywords=flask+mega+tutorial&qid=1725951534&sprefix=flask+mega+tutoria%2Caps%2C293&sr=8-1)

# note

> In Python, a subdirectory that includes a **init**.py file is considered a package, and can be imported. When you import a package, the **init**.py executes and defines what symbols the package exposes to the outside world.

```bash
# Flask needs to be told how to import application. by setting the FLASK_APP environment variable:
$ export FLASK_APP=practice.py

# after that, I can run flask app by type below.
$ flask run
```

```python
class User(db.Model):
    id: so.Mapped[int] = so.mapped_column(primary_key=True)
    username: so.Mapped[str] = so.mapped_column(sa.String(64), index=True, unique=True)
    email: so.Mapped[str] = so.mapped_column(sa.String(120), index=True, unique=True)
    password_hash: so.Mapped[Optional[str]] = so.mapped_column(sa.String(256))

    def __repr__(self):
        return "<User {}>".format(self.username)

# __repr__ method do below.
>>> from app.models import User
>>> u = User(username='susan', email='susan@example.com')
>>> u
<User susan>
```

```bash
$ flask db init
# makes migrations folder on app
# it's like git init

$ flask db migrate -m "users table"
# this generate a migration automatically. if there is no previous database, the automatic migration will add the entire User model to the migration script.
# i'ts like commiit on git

$ flask db upgrade
# this to actually make change to the database. it's like pusing git to the github.
```

```python
>>> app.app_context().push()
>>> u = User(username="john", email="john@example.com")
>>> db.session.add(u)
>>> db.session.commit()
>>> u
<User john>
>>> u2 = User(username="susan", email="susang@example.com")
>>> db.session.add(u2)
>>> db.session.commit()
>>> query = sa.select(User)
>>>
>>> users = db.session.scalars(query).all()
>>> users
[<User john>, <User susan>]

# get all users in reverse alphabetical order
>>> query = sa.select(User).order_by(User.username.desc())
>>> db.session.scalars(query).all()
[<User susan>, <User john>]

# get all users that have usernames starting with "s"
>>> query = sa.select(User).where(User.username.like('

Grinberg, Miguel. The New And Improved Flask Mega-Tutorial (2024 Edition) (p. 81). Miguel Grinberg. Kindle Edition.
```

> Q1. In User, posts is hinted WriteOnlyMapped["Post"]. is it ok not ["Post"] but just [Post]?

> A1. You can indeed use so.Mapped[Post] instead of so.Mapped["Post"] in many cases, especially if you are defining the Post class before using it in User. However, if Post is defined after User, Python's name resolution will not find Post because it hasn't been defined yet. Using the string form "Post" allows Python to defer the resolution of the Post class name until later. This is called a "forward reference," and it's useful when the referenced class is defined later in the file.

> Q2. WriteOnlyMApped is used because User is one and Post is many type of relationship in this situation?

> A2. The so.WriteOnlyMapped hint is used to indicate that the relationship is one-to-many, as you correctly pointed out. The posts attribute in User is a collection of Post objects (since a user can author multiple posts), and this is typically implemented as a one-to-many relationship in SQLAlchemy. However, WriteOnlyMapped suggests that you intend this relationship to be write-only, meaning you don't expect to read or access the related Post objects directly through this attribute. If you want a read-write relationship (which is common), you would simply use so.Mapped, not so.WriteOnlyMapped.

After configure practice.py like below,

```python
import sqlalchemy as sa
import sqlalchemy.orm as so

from app import app, db
from app.models import Post, User


@app.shell_context_processor
def make_shell_context():
    return {"sa": sa, "so": so, "db": db, "User": User, "Post": Post}

```

Using command `flask shell` will make python interpreter to understand flask things such like above.
With no `flask shell` but `python` command, then you need to import things and push to do things like below.

```python
>>> from app import app, db
>>> from app.models import User, Post
>>> import sqlalchemy as sa
>>> app.app_context().push()
```
