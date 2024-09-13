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
```
