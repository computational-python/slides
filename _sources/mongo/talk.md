# mongodb

## a nosql database

### notes


---

layout: false

## What is mongodb

* a document-oriented database
* maps more naturally to Python objects than SQL

## Concepts

* We have a mongodb server
    - can have local running on default port


## What we use

* pymongo
* mongoengine
* studio3T (formally robomongo)

---

## Mongo from the command-line

Start the mongo shell
~~~
$ mongo
>
~~~

* `use mylib` will activate (create) a database with name 'mylib'
    ~~~
    > use mylib
    switched to db mylib
    ~~~
* the global handle `db` is used to reference the active database
    ~~~
    > db
    mylib
    ~~~

---

* a database is made of collections
    ~~~
    > show collections
    >
    ~~~
    (initially empty, no collections exist)

* Create or update the database collection by inserting entries in JSON notation
    ~~~
    > db.books.insert({'title': 'Utvandrarna', 'authors': ['Moberg, Villhelm']})
    WriteResult({ "nInserted" : 1 })
    > db.books.insert({'title': 'Roseanna', 'authors': ['Sjövall, Maj', 'Walöö,  Per'})
    WriteResult({ "nInserted" : 1 })
	> db.books.count()
    2
    ~~~
    ~~~
    > show collections
    books
    ~~~

---

* Search
    ~~~
    > db.books.find().pretty()
    {
        "_id" : ObjectId("652cf41f59b374200cb603e5"),
        "title" : "Utvandrarna",
        "authors" : [
            "Moberg, Villhelm"
        ]
    }
    {
        "_id" : ObjectId("652cf7d059b374200cb603e6"),
        "title" : "Roseanna",
        "authors" : [
            "Sjövall, Maj",
            "Walöö, Per"
        ]
    }
    ~~~

---

## Pymongo

* The mongo shell enables interaction with the database using javascript
* We prefer Python: Pymongo to the rescue

~~~
>>> client = pymongo.MongoClient()
>>> client
MongoClient(host=['localhost:27017'], document_class=dict, tz_aware=False, connect=True)
~~~
* Select database
~~~
>>> db = client.mylib
>>> db.list_collection_names()
['books']
~~~
* Query collection
~~~
>>> db.books.count_documents({})
1
>>> list(db.books.find())
[{'_id': ObjectId('652cf41f59b374200cb603e5'), 'title': 'Utvandrarna', 'authors': ['Moberg, Villhelm']}, {'_id': ObjectId('652cf7d059b374200cb603e6'), 'title': 'Roseanna', 'authors': ['Sjövall, Maj', 'Walöö, Per']}]
~~~
---

## Mongongine

* Connect to server (locally running mongod on port  xxx) and give a database
  name
* An ODM (object document mapper) allows us to map Python objects to database
  entries. The equivalent for SQL-databases is called ORM (object relational mapper), see SQLAlchemy

~~~
>>> import mongoengine
>>> client = mongoengine.connect('mylib')
>>> client
MongoClient(host=['localhost:27017'], document_class=dict, tz_aware=False, connect=True, read_preference=Primary(), uuidrepresentation=3)
~~~

~~~
>>> db = client.get_database('mylib')
>>> db.list_collection_names()
['books']
~~~
~~~
>>> db.books
Collection(Database(MongoClient(host=['localhost:27017'], document_class=dict, tz_aware=False, connect=True, read_preference=Primary(), uuidrepresentation=3), 'mylib'), 'books')
~~~

~~~
>>> db.books.find_one()
{'_id': ObjectId('652cf41f59b374200cb603e5'), 'title': 'Utvandrarna', 'authors': ['Moberg, Villhelm']}
>>> 
~~~

---

Now with Python objects

~~~
>>> from mongoengine import Document, StringField, LisField
>>> class Book(Document):
...     title = StringField(required=True)
...     authors = ListField(StringField())
...     meta = {'db_alias': 'default', 'collection': 'books'}
...
...     def __repr__(self):
...         return f"Book(title='{self.title}')"
~~~
~~~
>>> mongoengine.register_connection('default', 'mylib')
>>> Book.objects
[Book(title='Utvandrarna'), Book(title='Roseanna')]
~~~

---

### Making a separate class for authors

~~~
class Author(Document):
    last = StringField(required=True)
    first = StringField(required=True)
    meta = {'db_alias': 'default', 'collection': 'authors'}

    def __repr__(self):
        return f'Author(last="{self.last}", first="{self.first}")'
~~~
~~~
>>> (mn := Author(last='Niemi', first='Mikael'))
Author(last="Niemi", first="Mikael")
~~~
~~~
>>> Book(title = 'Koka björn', authors=[Author(last='Niemi', first='Mikael')]).save()
Book(title='Koka björn')
~~~
