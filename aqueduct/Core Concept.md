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

Notice that we didn't have to override  **`handle`**  in  `ResourceController`. A  `ResourceController`  implements this method to call one of our  **_operation** methods_. An operation method - like  `getAllHeroes`  and  `getHeroByID`  - must have an  `Operation`annotation. The named constructor  `Operation.get`  means these methods get called when the request's method is GET. An operation method must also return a  `Future<Response>`.

`getHeroByID`'s annotation also has an argument - the name of our path variable  `id`. If that path variable exists in the request's path,  `getHeroByID`  will be called. If it doesn't exist,







<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MDM5NTk5MTMsMTc5MzI0Mjg1NywtNT
Y4NTM5ODUsMjA4NjAwODgwNCwxNTU5MzExMjQxXX0=
-->