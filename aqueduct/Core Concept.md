# Core Concepts

## Resources
Resources are the things your application exposes through its HTTP API.
In HTTP, resource means data which is the one being transferred and manipulated.

-   GET — Get a list of resources or single resource
-   POST — Create new resource
-   PUT — Update existing resource
-   DELETE — Delete a resource

## Routing
The route /organizations/:id will match the paths /organizations/1, /organizations/2, and so on.



## Controllers
Controllers are objects that handle requests. For example, a controller might fetch rows from a database and send them to the client in the response body. Another controller might verify the username and password of a request's Authorization header are valid. **Controllers are linked together to form a series of actions to take for a request. These linked together controllers are called a _channel_.**

There are two flavors of controllers. 
- ### _endpoint controller_  
	 performs operations on a resource or resource collection, and always sends a response. Endpoint controllers  _fulfill_  requests by returning the state of a resource or by changing the state of a resource. You write most of your application-specific logic in endpoint controllers.

- ### _middleware controller_  
	takes an action for a request, but isn't responsible for fulfilling the request. Middleware controllers can do many different things and are often reusable in many channels. Most often, a middleware controller validates something about a request before it reaches an endpoint controller. Middleware controllers can send a response for a request, and doing so prevents any other controller in that channel from handling the request.

A channel must have exactly one endpoint controller. It can be preceded by zero or more middleware controllers. See the guides on  [Controllers](https://aqueduct.io/docs/http/controller/)  and  [ResourceControllers](https://aqueduct.io/docs/http/resource_controller/)  for usage details.



### @Operation.get 

**_a member of ResourceController_**
- **implement handle method automatically**
we didn't have to override  **`handle`**  in  `ResourceController`. A  `ResourceController`  implements this method to call one of our  **_operation** methods_. An operation method - like  `getAllHeroes`  and  `getHeroByID`  - must have an  **`Operation`** annotation. The named constructor  **`Operation.get`**  means these methods get called when the request's method is **GET**. An operation method must also return a  `**Future<Response>**`.

`getHeroByID`'s annotation also has an argument - the name of our path variable  `id`. If that path variable exists in the request's path,  `getHeroByID`  will be called. If it doesn't exist,

```dart
class HeroesController extends ResourceController {
  final _heroes = [
    {'id': 11, 'name': 'Mr. Nice'},
    {'id': 12, 'name': 'Narco'},
    {'id': 13, 'name': 'Bombasto'},
    {'id': 14, 'name': 'Celeritas'},
    {'id': 15, 'name': 'Magneta'},
  ];
  @Operation.get()
  Future<Response> getAllHeroes() async {
    return Response.ok(_heroes);
  }
  @Operation.get('id')
  Future<Response> getHeroByID() async {
    final id = int.parse(request.path.variables['id']);
    final hero = _heroes.firstWhere((hero) => hero['id'] == id, orElse: () => null);
    if (hero == null) {
      return Response.notFound();
    }
    return Response.ok(hero);
  }
}
```

## Request Binding @bind

In our  `getHeroByID`  method, we make a dangerous assumption that the path variable 'id' can be parsed into an integer. If 'id' were something else, like a string,  `int.parse`  would throw an exception. When exceptions are thrown in operation methods, the controller catches it and sends a 500 Server Error response. 500s are bad, they don't tell the client what's wrong. A 404 Not Found is a better response here, but writing the code to catch that exception and create this response is cumbersome.

Instead, we can rely on a feature of operation methods called  _request binding_. An operation method can declare parameters and  _bind_  them to properties of the request.

```dart
@Operation.get('id')
Future<Response> getHeroByID(@Bind.path('id') int id) async {
  final hero = _heroes.firstWhere((hero) => hero['id'] == id, orElse: () => null);

  if (hero == null) {
    return Response.notFound();
  }
  return Response.ok(hero);
}
```


## Using ManagedContext to Connect to a Database


The interface to a database from Aqueduct is an instance of  `ManagedContext`  that contains the following two objects:

-   a  `ManagedDataModel`  that describes your application's data model
-   a  `PersistentStore`  that manages a connection to a single database

A **`ManagedContext`** uses these two objects to coordinate moving data to and from your application and a database. A **`Query<T>`** object uses a context's persistent store to determine **which database to send commands to**, and a **data model to map database rows to objects** and vice versa.

A context, like all service objects, is created in **`ApplicationChannel.prepare`**.

```dart
class MyApplicationChannel extends ApplicationChannel {
  ManagedContext context;

  @override
  Future prepare() async {
    var dataModel = ManagedDataModel.fromCurrentMirrorSystem();
    var psc = PostgreSQLPersistentStore.fromConnectionInfo(
   "my_app_name_user", "password", "localhost", 5432, "my_app_name");

    context = ManagedContext(dataModel, psc);
  }
}
```

The `ManagedDataModel.fromCurrentMirrorSystem` finds every **`ManagedObject<T>`** subclass in your application's code. Optionally, you may specify an exact list:

```dart
var dataModel = ManagedDataModel([User, Post, Friendship]);
```


## Configuration Files

following  `Configuration`  subclass:

```dart
class TodoConfiguration extends Configuration {
  TodoConfiguration(String fileName) : super.fromFile(fileName);

  DatabaseConnectionConfiguration database;
  String apiBaseURL;

  @optionalConfiguration
  int identifier;
}
```

This would read a YAML file like this:

```yaml
database:
  username: fred
  password: fredspassword
  host: db.myapp.com
  port: 5432
  databaseName: fredsdb
apiBaseURL: /api
identifier: 2
```

If required properties are omitted from the YAML file being read, application startup will fail and throw an informative error.

You may use `Configuration`s to read values from environment variables. In `config.yaml`, use a **`$`-prefixed** ***environment variable*** name instead of a value:

```yaml
database: $DATABASE_CONNECTION_URL
apiBaseURL: /api
```
> $: 指向 ENV variable

 

### Configuration Conventions and Deployment Options

Aqueduct uses two configuration files for a project: `config.yaml` and `config.src.yaml`. 

- The latter is the _configuration source file_. 
  The configuration source file declares key-value pairs that will be used when running the application tests. Deployed instances use `config.yaml`.

This pattern is used for two reasons:

-   It is the template for the  `config.yaml`  that will be read on deployed applications, providing documentation for your application's configuration.
-   It has the configuration values used during testing to inject mock dependencies.

For example, a production API instance might have the following  `config.yaml`  file with connection info for a production database:

```yaml
database: postgres://app_user:$%4jlkn#an*@mOZkea2@somedns.name.com:5432/production_db
```

Whereas  `config.src.yaml`  would have connection info for a local, test database:

```yaml
database: postgres://test:test@localhost:5432/temporary_db
```







<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgwNTIxOTg0NiwzNTg5OTY5MzEsLTE2OT
Y5MzM4MTEsMTU2MjU0NTA0MSwxMjQxMTE2Mjg1LC00MTA0ODA0
OSwtMzE4MTE1NjYxLDQ3Mjk5OTQxMSw2MjAwODIyMzgsLTg1Mj
A1NjU0NCwtNjcxNjkzMzIzLC0yMDE2NzEzNDAxLC00NDM2NDY3
OCwtMTE4OTg0OTA1LDUzODkyNDcxMiwxNzkzMjQyODU3LC01Nj
g1Mzk4NSwyMDg2MDA4ODA0LDE1NTkzMTEyNDFdfQ==
-->