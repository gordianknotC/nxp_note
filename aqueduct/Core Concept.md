# Core Concepts

## Resources
Resources are the things your application exposes through its HTTP API.


## Routing
The route /organizations/:id will match the paths /organizations/1, /organizations/2, and so on.



## Controllers
Controllers are objects that handle requests. For example, a controller might fetch rows from a database and send them to the client in the response body. Another controller might verify the username and password of a request's Authorization header are valid. **Controllers are linked together to form a series of actions to take for a request. These linked together controllers are called a _channel_.**









<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU1OTMxMTI0MV19
-->