
## Transient Properties

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

## Example: One-to-Many Relationship

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



## Example: Many-to-Many Relationship

Hierarchical relationships follow the same rules as all other relationship, but declare the foreign key property and the inverse in the same type.

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



## ManagedObject Serialization and Deserialization

In this guide, you will learn how  `ManagedObject<T>`s are read from HTTP request bodies and written to HTTP response bodies.



## Basic Behavior

A  `ManagedObject<T>`  can be converted to and from  `Map<String, dynamic>`  objects (which can be encoded and decoded into request or response body using JSON or some other data format). 

- **`readFromMap`:**
  To decode a  `ManagedObject`  into a  `Map`, use the instance method  
	```dart
	final object = MyManagedObject();
	object.readFromMap({
	  "key": "value"
	});
	```
	When decoding, the value for each key in the map is assigned to the managed object property of the same name. 

	> ⚡ If a key exists in the map and the managed object does not have a property of the same name, a `ValidationException` will be thrown (this sends a **400 Bad Request response if uncaught**).


- **`asMap`:**
  To encode a map into a managed object is converted, use the instance method  
	```dart
	final object = MyManagedObject();
	final map = object.asMap();
	```
	
|dart type| serialized type |
|--|--|
| int | number(int) |
| double| number(double) |
| String| string(String) |
| DateTime| ISO 8601 Timestamp (`String`) |
| bool | boolean(bool) |
| Document | map or list (`Map<String, dynamic>` or `List<dynamic>`) |
| enum |string(String) |
| RelationShip | list or maps |
 
Both  `asMap`  and  `readFromMap`  are inherited methods from  `Serializable`. As a  `Serializable`, a managed object can be  [bound to a request body in an  `ResourceController`  operation method](https://aqueduct.io/docs/http/resource_controller/)  and  [encoded as a response body object](https://aqueduct.io/docs/http/request_and_response/). For example:

```dart
class UserController extends ResourceController {
  @Operation.post()
  Future<Response> createUser(@Bind.body() User user) async {
    var query = Query<User>(context)
      ..values = user;

    final newUser = await query.insert();

    return Response.ok(newUser);
  }
}
```
> ⚡ **@Bind.body()**
> Binds an HTTP request body to an [ResourceController] property or operation method argument.
> _

> ⚡ **Autoincrementing Properties**
> Properties that are autoincrementing will never be read from a map from `readFromMap`.
> _


## Behavior of Null Values

A property of a **managed** object **can be null for two reasons:** 
	- the value is actually null
	- or the value is not available
For example, when you create a new instance of a managed object, none of its values are available (the object is empty). When encoding an object into a map, only the available values are included and the keys for any unavailable properties are omitted:


## Behavior of Transient Properties

By default, transient properties - those declared in the **managed** object subclass, **not the table definition** - are  **_not_**  included in an object's  `asMap()`. The  `Serialize`annotation allows a transient property to be included in this map.





<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM0NzQ3MjYzNywtMTI4NzQ2MDk4MCw5Mz
UxNjU3NjcsMTk1NDk3ODg1NywzNDQwNTE4OTksNjgzMzIyODcy
LC0xNzAxMTQyNTk0LC0xMDM1MzE2NjkyLDIwNzAyMjc0MDgsMT
M3Mjc1ODY5NiwxMjQ0MDU1ODEsMjA4OTcyNjQzNF19
-->