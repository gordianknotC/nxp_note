
### Transient Properties

Properties declares in a managed object subclass are called  _transient_  because they are not stored in a database. For example, consider an  `Author`  type that stores first and last name as separate columns. Instead of redundantly storing a 'full name' in the database

```dart
class Author extends ManagedObject<_Author> implements _Author {
  String get name => "$firstName  $lastName";
  set name(String fullName) {
    firstName = fullName.split(" ").first;
    lastName = fullName.split(" ").last;
  }
}
```
By default, a transient property is ignored when reading an object from a request body or writing the object to a response body (see the guide on  [serialization](https://aqueduct.io/docs/db/serialization/)  for more details). 

> ⚡ You can annotate a transient property with  `Serialize`   so that it is able to be **read from a request body**, **written to a response body**, or both. For example:

```dart
class Author extends ManagedObject<_Author> implements _Author {
  @Serialize()
  String get name => "$firstName  $lastName";

  @Serialize()
  set name(String fullName) {
    firstName = fullName.split(" ").first;
    lastName = fullName.split(" ").last;
  }
}
```

### Example: One-to-Many Relationship

#### An author has many books:
```dart
class Author extends ManagedObject<_Author> implements _Author {}
class _Author {
  @primaryKey
  int id;

  String name;

  ManagedSet<Book> books;
}

class Book extends ManagedObject<_Book> implements _Book {}
class _Book {
  @primaryKey
  int id;

  String name;

  @Relate(#books)
  Author author;
}
```
#### To insert an author and a book associated with that author:
```dart
final authorQuery = Query<Author>(context)
  ..values.name = "Fred";
final author = await authorQuery.insert();

final bookQuery = Query<Book>(context)
  ..values.name = "Title"
  ..values.author.id = author.id;
final book = await bookQuery.insert();
```
#### To fetch authors and their books:
```dart
final query = Query<Author>(context)
  ..join(set: (a) => a.books);
final authors = await query.fetch();
```

> ⚡ **set:** indicates **ManagedSet** which is an list of int referenced to a target table

#### To fetch a book and their full author object:
```dart
final query = Query<Book>(context)
  ..where((b) => b.id).equalTo(1)
  ..join(object: (a) => a.author);
final books = await query.fetch();
```

> ⚡ **object:** indicates a **Relate** object which referenced from author



### Example: Hierarchical Relationships (Self Referencing)

Hierarchical relationships follow the same rules as all other relationship, but declare the foreign key property and the inverse in the same type.
```dart
class Person extends ManagedObject<_Person> implements _Person {}
class _Person {
  @primaryKey
  int id;

  String name;

  ManagedSet<Person> children;

  @Relate(#children)
  Person parent;
}
```



### Example: Many-to-Many Relationship

```dart
class Team extends ManagedObject<_Team> implements _Team {}
class _Team {
  @primaryKey
  int id;

  String name;
  ManagedSet<TeamPlayer> teamPlayers;
}

// This type is a join table
class _TeamPlayer extends ManagedObject<_TeamPlayer> implements _TeamPlayer {}
class _TeamPlayer {
  @primaryKey
  int id;  

  @Relate(#teamPlayers)
  Team team;

  @Relate(#teamPlayers)
  Player player;
}

class Player extends ManagedObject<_Player> implements _Player {}
class _Player {
  @primaryKey
  int id;

  String name;
  ManagedSet<TeamPlayer> teamPlayers;
}
```





<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA3MDIyNzQwOCwxMzcyNzU4Njk2LDEyND
QwNTU4MSwyMDg5NzI2NDM0XX0=
-->