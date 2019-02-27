
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
















































<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc2MDQyMjkyXX0=
-->