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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMzA4MjM3ODIsLTY3MTY5MzMyMywtMj
AxNjcxMzQwMSwtNDQzNjQ2NzgsLTExODk4NDkwNSw1Mzg5MjQ3
MTIsMTc5MzI0Mjg1NywtNTY4NTM5ODUsMjA4NjAwODgwNCwxNT
U5MzExMjQxXX0=
-->