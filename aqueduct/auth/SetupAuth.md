
# Creating AuthServers to Authenticate and Authorize

> ⚡ An  `AuthServer`  is a service that handles 
> - creating 
> - verifying and 
> - refreshing 
> authorization tokens. You create an  `AuthServer`  in your application channel and inject into types that deal with authorization. 

This types include:

-   `Authorizer`: middleware controller that protects endpoint controllers from unauthorized access
-   `AuthController`: endpoint controller that grants **access tokens**
-   `AuthCodeController`: endpoint controller that grants authorization codes to be **exchanged** for access tokens

An  `AuthServer`  must persist the data it uses and creates - like client identifiers and access tokens. Storage is often performed by a database, but it can be in memory, a cache or some other medium. Because of the many different storage mediums, an  `AuthServer`  doesn't perform any storage itself - it relies on an instance of  `AuthServerDelegate`  specific to your application. This allows storage to be independent of verification logic.



## Creating Instances of AuthServer and AuthServerDelegate

`AuthServerDelegate`  is an interface that an  `AuthServer`  uses to handle storage of client identifiers, tokens and other authorization artifacts. An  `AuthServer`  must be created with a concrete implementation of  `AuthServerDelegate`. Aqueduct contains a concrete implementation of  `AuthServerDelegate`  that uses the ORM. It is highly recommended to use this implementation instead of implementing your own storage because it has been thoroughly tested and handles cleaning up expired data correctly.

This concrete implementation is named  `ManagedAuthDelegate<T>`. It exists in a sub-package of Aqueduct and must be explicitly imported. Here's an example of creating an  `AuthServer`  and  `ManagedAuthDelegate<T>`:














































<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3MTQ4NDA5MiwxNzYwNDIyOTJdfQ==
-->