# Issue Access Tokens with AuthController

An application using Aqueduct's Auth framework must have endpoints to exchange credentials for access tokens. While a developer could implement these endpoints themselves and talk directly to an  `AuthServer`, the OAuth 2.0 specification is where happiness goes to die. Therefore, there exist two  `Controller`s in Aqueduct that handle granting and refreshing authorization tokens -  `AuthController`  and  `AuthCodeController`.

### Issue, Refresh and Exchange Tokens with AuthController

An  `AuthController`  
- grants access tokens 
- refreshes tokens 
- exchanges authorization codes obtained from  `AuthCodeController`  for access tokens.

Using an  `AuthController`  in an application is straightforward - hook it up to a  `Router`  and pass it an  `AuthServer`.

```dart
@override
Controller get entryPoint {
  final router = Router();
  router
    .route("/auth/token")
    .link(() => AuthController(authServer));
  return router;
}
```

To grant an **access token**, a client application sends a HTTP  **`POST`**  to the controller. The request must have:

-   an Authorization header with the **Client ID** and **Client Secret** (if one exists) and,
-   a  **`x-www-form-urlencoded`**  body with the username and password of the authenticating user.

The body must also contain the key-value pair  **`grant_type=password`**. For example, the following Dart code will initiate successful authentication:

#### client request:
```dart
var clientID     = "com.app.demo";
var clientSecret = "mySecret";
var body = "username=bob@stablekernel.com&password=foobar&grant_type=password";
var clientCredentials = Base64Encoder().convert("$clientID:$clientSecret".codeUnits);
var response          = await http.post(
  "https://stablekernel.com/auth/token",
  headers: {
    "Content-Type": "application/x-www-form-urlencoded",
    "Authorization": "Basic $clientCredentials"
  },  body: body);
```

If the OAuth 2.0 client ID is public - that is, it does not have a client secret - the secret is omitted from the authorization header:

```dart
// Notice that the separating colon (:) is still present.
var clientCredentials = Base64Encoder().convert("$clientID:".codeUnits);
```

The response to a password token request is a JSON body that follows the OAuth 2.0 specification:
#### server response
```dart
{
  "access_token": "..."
  "refresh_token": "...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

The  `expires_in`  field is a computed property based on the delta of the issue date and expiration date. You should avoid manually editing the values for the columns  `issuedate`  and  `expirationdate`

> Tokens are refreshed through **the same** endpoint, but with a payload that only contains the **refresh token** and  **`grant_type=refresh_token`**.

```dart
var body = "username=bob@stablekernel.com&password=foobar"
"&grant_type=refresh_token&refresh_token=kjasdiuz9u3namnsd";
```

See  [Aqueduct Auth CLI](https://aqueduct.io/docs/auth/cli/)  for more details on creating OAuth 2.0 client identifier and secrets.

If an Aqueduct application is using scope, an additional  `scope`  parameter can contain a space-delimited list of requested authorization scope. Only allowed scopes are returned and granted, and if no scopes are allowed then the request fails. If scope is provided, granted scope will be available in the response body.

It is important that an  `Authorizer`  _must not_  protect instances of  `AuthController`. The Authorization header is parsed and verified by  `AuthController`.

Once granted, an access token can be used to pass  `Authorizer.bearer()`s in the application channel.



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzODY4NjY4NiwzNzQxNjkyMzQsMTc5NT
c5NTMxNSwtMTczNDg1OTM0MSwxMDIyMzY5NDc0LDE3MzM1NTA5
OTNdfQ==
-->