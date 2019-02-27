### Issue Authorization Codes with AuthCodeController

An  `AuthCodeController`  manages the OAuth 2.0 authorization code flow. The authorization code flow is used when an Aqueduct application **allows third party** applications access to authorized resources.

Let's say you've built an Aqueduct application that allows people to store notes for themselves. Now, a friend approaches you with their application that is a to-do list. Instead of building their own note-taking feature, your friend wants users of their application to access the notes the user has stored in your application. While trustworthy, you don't want your friend to have access to the username and passwords of your subscribers.

Your friend adds a link to their application that takes the user to an HTML page hosted by your server. The user enters their credentials in this page, which sends a  `POST`  request to your server. Your server responds by redirecting the user's browser back into your friend's application. An  _authorization code_  is included in the query string of the redirect URL.

Your friend's application parses the code from the URL and sends it to  _their_  server. Behind the scenes, their server exchanges this code with your server for an access token.

An  `AuthCodeController`  responds to both  `GET`  and  `POST`  requests. When issued a  `GET`, it serves up a HTML page with a login form. This login form's submit action sends a  `POST`  to the same endpoint with the username and password of the user. Upon success, the response from the  `POST`  is a 302 redirect with an authorization code.

Setting up an  `AuthCodeController`  is nearly as simple as setting up an  `AuthController`, but requires a function that renders the HTML login form. Here's an example:

```dart
@override
Controller get entryPoint {
  final router = Router();
  router
    .route("/auth/code")
    .link(() => AuthCodeController(
      authServer, renderAuthorizationPageHTML: renderLogin));

  return router;
}

Future<String> renderLogin(
    AuthCodeController requestingController,
    URI requestURI,
    Map<String, String> queryParameters) {
  var html = HTMLRenderer.templateWithSubstitutions(
    "web/login.html", requestURI, queryParameters);

  return html;
}
```

It is important that all values passed to HTML rendering function are sent in the form's query parameters - they contain necessary security components and scope information.

When your friend's application links to your login page -  `GET /auth/code`  - they must include three query parameters:  `state`,  `client_id`,  `response_type`. They may optionally include  `scope`.

https://stablekernel.com/auth/code?client_id=friend.app&response_type=code&state=87uijn3rkja

The value of  `client_id`  must be created specifically for your friend's application and stored in your database. (See more on generating client identifiers with  `aqueduct auth`  in  [Aqueduct Auth CLI](https://aqueduct.io/docs/auth/cli/).) The  `response_type`  must always be  `code`. The  `state`  must be a value your friend's application creates - it is often some random value like a session cookie.

When a user of your friend's application goes through this process, they are redirected back into your friend's application. Both the generated authorization code and the value for  `state`  will be query parameters in the URL. That redirect URL will look like:

https://friends.app/code_callback?code=abcd672kk&state=87uijn3rkja

The redirect URL is pre-determined when generating the client identifier with  `aqueduct auth`.

Your friend's application verifies that  `state`  matches the  `state`  they sent in  `GET /auth/code`. They then send the  `code`  to their server. The server then exchanges this code with your server by issuing a  `POST`  to an  `AuthController`  -  _NOT_  the  `AuthCodeController`  - with the following  `application/x-www-form-urlencoded`  body:

grant_type=authorization_code&code=abcd672kk

An access token will be returned to the server which your friend then stores in their database. Whenever one of their users makes a request that requires accessing your application's data, they will execute requests with that access token.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxNDU3MjkzMDRdfQ==
-->