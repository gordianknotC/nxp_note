# Aqueduct
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Aqueduct](#aqueduct)
  - [Auth 2.0:](#auth-20)
  - [Overview](#overview)
    - [Tasks](#tasks)
  - [What is OAuth 2.0?](#what-is-oauth-20)
  - [Auth Code Controller:](#auth-code-controller)
  - [Issue Authorization Codes with AuthCodeController](#issue-authorization-codes-with-authcodecontroller)
  - [Auth Controller:](#auth-controller)
  - [Issue Access Tokens with AuthController](#issue-access-tokens-with-authcontroller)
  - [CRUD:](#crud)
  - [Inserting, Updating, Deleting and Fetching Objects](#inserting-updating-deleting-and-fetching-objects)
    - [Specifying Result Properties](#specifying-result-properties)
  - [Security Route:](#security-route)
  - [Securing Routes with Authorizer](#securing-routes-with-authorizer)
  - [Setup Auth:](#setup-auth)
  - [Creating AuthServers to Authenticate and Authorize](#creating-authservers-to-authenticate-and-authorize)
    - [Creating Instances of AuthServer and AuthServerDelegate](#creating-instances-of-authserver-and-authserverdelegate)
    - [Using ManagedAuthDelegate](#using-managedauthdelegate)
  - [Understand Authentication:](#understand-authentication)
    - [Understanding Login Authentication](#understanding-login-authentication)
  - [Codecs:](#codecs)
    - [Streaming Response Bodies](#streaming-response-bodies)
  - [Codecs and Content Types](#codecs-and-content-types)
    - [register codecs into CodeRegistery](#register-codecs-into-coderegistery)
    - [Compression with gzip](#compression-with-gzip)
  - [Serializable Objects](#serializable-objects)
    - [Serializable and OpenAPI Generation](#serializable-and-openapi-generation)
  - [🔥 Example: multipart/form-data](#-example-multipartform-data)
  - [Processing MIME multipart media types  #](#processing-mime-multipart-media-types--)
  - [Core Concept:](#core-concept)
  - [Core Concepts](#core-concepts)
    - [Resources](#resources)
    - [Routing](#routing)
    - [Controllers](#controllers)
    - [Request Binding @bind](#request-binding-bind)
    - [Using ManagedContext to Connect to a Database](#using-managedcontext-to-connect-to-a-database)
    - [Configuration Files](#configuration-files)
  - [DataBase:](#database)
  - [Example: One-to-Many Relationship](#example-one-to-many-relationship)
    - [Example: Hierarchical Relationships (Self Referencing)](#example-hierarchical-relationships-self-referencing)
  - [Example: Many-to-Many Relationship](#example-many-to-many-relationship)
  - [ManagedObject Serialization and Deserialization](#managedobject-serialization-and-deserialization)
  - [Basic Behavior](#basic-behavior)
  - [Behavior of Null Values](#behavior-of-null-values)
  - [Behavior of Transient Properties](#behavior-of-transient-properties)
  - [Auth](#auth)
  - [JSON:](#json)
  - [JSON Document Storage](#json-document-storage)
    - [JSON Columns in Relational Databases](#json-columns-in-relational-databases)
    - [The Document Data Type](#the-document-data-type)
    - [Basic Operations on Document Properties](#basic-operations-on-document-properties)
    - [Fetching Sub-documents](#fetching-sub-documents)
  - [Multipart:](#multipart)
    - [Multipart header](#multipart-header)
  - [Resource Controller:](#resource-controller)
  - [ResourceController](#resourcecontroller)
    - [Operation Methods](#operation-methods)
    - [Routing to a ResourceController](#routing-to-a-resourcecontroller)
    - [Request Bindings](#request-bindings)
    - [Other ResourceController Behavior](#other-resourcecontroller-behavior)
  - [Serving Files:](#serving-files)
  - [Serving Files and Caching](#serving-files-and-caching)
    - [FileController](#filecontroller)
    - [Caching](#caching)
    - [File Serving and Caching Outside of FileController](#file-serving-and-caching-outside-of-filecontroller)
  - [Validation:](#validation)
  - [Validating Data](#validating-data)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Auth 2.0:
## Overview

### Tasks

Aqueduct has types to manage authentication and authorization according to the  [OAuth 2.0 specification](https://tools.ietf.org/html/rfc6749).

You create an  **`AuthServer`  service object** for your application that **manages authentication** and **authorization logic**. 

An  `AuthServer`  requires a helper object that implements  **`AuthServerDelegate`**  to handle configuration and required data storage. Most often, this object is a  **`ManagedAuthDelegate<T>`**  that uses the Aqueduct ORM to manage this storage.

An  **`AuthServer`  service object** is injected into  **`Authorizer`  controllers** that protect access to controller channels. An  `AuthServer`  is also injected into  **`AuthCodeController`  and  `AuthController`**  to provide HTTP APIs for authentication.

The  `aqueduct auth`  command-line tool manages configuration - such as client identifier management - for live applications.

![Authorization Objects](https://aqueduct.io/docs/img/authobjects.png)



## What is OAuth 2.0?

Most applications have the concept of a user. To prevent just anyone from saying they are this user, the user has a password. When a user wants to use your application, they send their username and password to the server so it can ensure they are who they say they are.

The simple way to do this is to send the username and password in an Authorization header for every request. Two reasons this is bad: - 

- first, every time the user wants to do something, their password is sent in every request and that can be unsafe. 
- Second, any application that wants to keep the user logged in has to store that password - and that is unsafe.

In OAuth 2.0, the user gives their username and password to a client application **once**. 

- The application sends those credentials to a server
- The server checks the token, makes sure its not expired, and then lets the application's request go through. 
- The application doesn't have to store the password and doesn't have to ask the user for their password again.
- the server gives the application back an access token. 

> 📝 A access token is a long, random string that no one can guess. When the user wants to do more stuff, the application sends the **token** with **every request**, instead of the user's **password**. 

This **credential-for-token exchange** happens by sending a **POST** request to some endpoint, where the username and password are sent in the request body. Typically, an Aqueduct application's route for this is  `/auth/token`  and handled by an instance of  [AuthController](https://aqueduct.io/docs/auth/controllers/).

OAuth 2.0 makes a subtle distinction: a user and the application they are using are not the same thing. It's intuitive to think of a user as "making a request to the server", but in reality, the user makes a request to an application and  _the application_  makes the request to the server. The server  _grants_  the application access on behalf of the user. In other words, when a user enters their credentials into an application, the application goes to the server and says "Hey, this user said I can do stuff for them. Look, this is their secret password!"

This is an important distinction, because an **OAuth 2.0 server doesn't just verify users: it also verifies applications.** An application has a identifier that has been registered with the server. 

So, for an ecosystem that has a web client, an Android and an iOS app there would likely be three identifiers - one for each. 
> 📝 NOTE:
> - user identifier
> - app identifier
> - platform identifier

The client application usually stores that identifier in a database. This identifier is called a  **_client identifier_**. Client identifiers are added to Aqueduct applications with the  `aqueduct auth`  tool (see  [Aqueduct Auth CLI](https://aqueduct.io/docs/auth/cli/)).

When the user is logging in through an application, they submit their username and password. The user doesn't provide the client identifier - in fact, the user doesn't know it. 

The application sends a request with the user's username and password in the request body, and the client identifier in the Authorization header. All three have to check out for the server to give the application back a token. The full request in pseudo-code looks something like:


```dart
var request = HTTPRequest("/auth/token");
request.method = "POST";
request.contentType = "application/x-www-form-urlencoded";
request.authorization = Base64.encode("$clientID:");
request.body = {
  "username" : "bob@stablekernel.com",
  "password" : "supersecretstuff",
  "grant_type" : "password"
};
```

An access token can expire. How long it takes to expire is up to the server - Aqueduct defaults to 24 hours. At first glance, this means that the application would have to ask the user for a password again. But, tokens can also be refreshed. **Refreshing a token grants a brand new access token**, but **without** having to ask for the password. This is possible because an access token comes with a  **_refresh token_**. The refresh token is another long, random string. So, the JSON the server sends back when granting a token looks like this:

**The access token**
```json
{
  "access_token" : "Abca09zzzza2o2kelmzlli3ijlka",
  "token_type" : "bearer",
  "refresh_token" : "lkmLIAmooa898nm20jannnnnxaww",
  "expire_in" : 3600
}
```

The application hangs on to both an access token and a refresh token. **When the token expires, it will send the refresh token back to the server** to get a replacement access token. This is done through the same route that the access token came from -  `/auth/token`  - except the parameters are a bit different:

**The refresh token**
```dart
var request = HTTPRequest("/auth/token");
request.method = "POST";
request.contentType = "application/x-www-form-urlencoded";
request.authorization = Base64.encode("$clientID:");
request.body = {
  "refresh_token" : "lkmLIAmooa898nm20jannnnnxaww",
  "grant_type" : "refresh_token"
};
```

Exchanging a refresh token has the same response as the initial exchange for username and password - except a few values will have changed.

The verification and storage of authorization and authentication information is managed by an  [AuthServer](https://aqueduct.io/docs/auth/server/).



#### Other Methods for Obtaining Authorization

The method of getting a token above - sending a username and password to  `/auth/token`  - is just one of four possible methods OAuth 2.0 uses to authenticate a user. This particular one is called the  _resource owner password credentials grant_. A resource owner is a fancy word for a 'user'. We can shorten it up to just the **'password flow'**. It's probably the most common flow - mobiles applications and front-end web applications often use this flow. When you enter your credentials, the client application sends them directly to the server.

The other commonly used flow **prevents** the client application from ever **seeing the user's credentials**. For example, you might sign into Pivotal Tracker with your Google account. Your account on Pivotal Tracker **doesn't have** a password. Instead, it is **linked to** your Google account - which does. Pivotal Tracker never sees your Google password. When you login to Pivotal Tracker in this way, it takes you to Google's authentication page - owned and operated by Google. When you login successfully, Google gives Pivotal Tracker your token. Pivotal Tracker is now an application that can do things on your behalf.

This is called the  _authorization code grant_  - or just 'auth code flow'. An instance of  `AuthCodeController`  handles granting authorization codes. Once a code is received, it can be exchanged for a token via an  `AuthController`.











<!--stackedit_data:
eyJoaXN0b3J5IjpbNDAyMzgyMzE4LDIwNzIzMzA3OTgsNTMyMT
IwNTg2LDMxNzMyNjI0MiwxNzE3MjE5MDksLTYyODQwMTgwNyw0
MzYxMTI0NzBdfQ==
-->

## Auth Code Controller:

## Issue Authorization Codes with AuthCodeController

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
eyJoaXN0b3J5IjpbLTE2MTA1NzM4MTYsLTIxNDU3MjkzMDRdfQ
==
-->

## Auth Controller:
## Issue Access Tokens with AuthController

An application using Aqueduct's Auth framework must have endpoints to exchange credentials for access tokens. While a developer could implement these endpoints themselves and talk directly to an  `AuthServer`, the OAuth 2.0 specification is where happiness goes to die. Therefore, there exist two  `Controller`s in Aqueduct that handle granting and refreshing authorization tokens -  `AuthController`  and  `AuthCodeController`.

#### Issue, Refresh and Exchange Tokens with AuthController

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

##### client request:
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
##### server response
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



## CRUD:

## Inserting, Updating, Deleting and Fetching Objects

To send commands to a database - whether to fetch, insert, delete or update objects - you will create, configure and execute instances of  `Query<T>`. The type argument must be a subclass of  `ManagedObject`, which determines the table the query will operate on.

A query compiles and executes a SQL query and requires a  [ManagedContext](https://aqueduct.io/docs/db/connecting.db)  to determine the database to connect to. Here's an example of a  `Query<T>`  that fetches all instances of  `User`:

final query = Query<User>(context);
final allUsers = await query.fetch();

A  `Query<T>`  has four basic execution methods:  `fetch`,  `update`,  `insert`,  `delete`.

-   `fetch`  will retrieve data from a database (it is equivalent to the SQL operation  `SELECT`).
-   `update`  will modify existing data in a database (it is equivalent to the SQL operation  `UPDATE`).
-   `insert`  will add new data to a database (it is equivalent to the SQL operation  `INSERT`).
-   `delete`  will remove data from a database (it is equivalent to the SQL operation  `DELETE`).

A  `Query<T>`  has many configurable properties. These properties will impact which objects get fetched, the data that gets sent to the database, the order that data is returned in, and so on.

In the following sections, assume that a  `User`  managed object subclass exists that is declared like so:

class User extends ManagedObject<_User> implements _User {}
class _User {
  @primaryKey
  int id;

  @Column(indexed: true)
  String email;

  String name;
}

#### Inserting Data with a Query

To insert data with a query, you create a new  `Query<T>`  object, configure its  `values`  property and then call its  `insert()`  method.

final query = Query<User>(context)
  ..values.name = "Bob"
  ..values.email = "bob@stablekernel.com";  

final user = await query.insert();  

The  `values`  of a  `Query<T>`  is an instance of  `T`  (the managed object type you are inserting). You can configure individual properties of  `values`, or you can assign  `values`  to an instance you have created elsewhere:

final userValues = User()
  ..name = "Bob"
  ..email = "bob@stablekernel.com";
final query = Query<User>(context)..values = userValues;
final user = await query.insert();  

Either way, this query is translated into the following SQL:

INSERT INTO _user (name, email) VALUES ('Bob', 'bob@stablekernel.com') RETURNING id, name, email;

Notice that only the values set on the  `values`  object are included in the SQL INSERT query. In this example, both  `name`  and  `email`  were set, but not  `id`  and therefore only  `name`  and  `email`  were included in the query. (In this case, the primary key is auto-incrementing and the database will generate it.)

Values that are explicitly set to  `null`  will be sent as  `NULL`. For example, consider the following  `Query<T>`  and its SQL:

var query = Query<User>(context)
  ..values.name = null
  ..email = "bob@stablekernel.com";

// INSERT INTO _user (name, email) VALUES (NULL, 'bob@stablekernel.com') RETURNING id, name, email;
await query.insert();

An insert query will return a managed object that represents the row that is inserted.

Prefer the Inserted Object

After you insert an object, you should prefer to use the object returned by an insert query rather than the values you used to populate the query. The object returned from the query will be an accurate representation of the database row, while the object used to build the query may be incomplete or different. For example, an auto-incrementing primary key won't be available in your query-building instance, but will in the object returned from the successful query.

There is one difference to note when choosing between assigning an instance to  `values`, or configuring the properties of  `values`. In an instance you create, a relationship property must be instantiated before accessing its properties. When accessing the relationship properties of  `values`, an empty instance of the related object is created immediately upon access.

final employee = Employee()
  ..manager.id = 1; // this is a null pointer exception because manager is null

final query = Query<Employee>(context)
  ..values.manager.id = 1; // this is OK because of special behavior of Query

Once you assign an object to  `values`, it will adopt the behavior of  `values`  and instantiate relationships when accessed. Also note that after assigning an object to  `values`, changes to the original object are not reflected in  `values`. In other words, the object is copied instead of referenced.

For simple insertions and for inserting more than one object, you can use the methods on the context:

final context = ManagedContext(...);
final bob = User()..name = "Bob";
final jay = User()..name = "Jay";

final insertedObject = await context.insertObject(bob);
final insertedObjects = await context.insertObjects([bob, jay]);

#### Updating Data with a Query

Updating rows with a  `Query<T>`  is similar to inserting data: you set the  `Query.values`  for properties you want to change. The type parameter for the  `Query<T>`  indicates which database table will get updated when the query is executed.

An update query can - and likely should - be restricted to a single row or subset of rows. This is done by configuring the  `Query.where`  property - which gets translated into the  _where clause_  of the SQL command. Here's an example:

// A Query that will change any user's whose name is 'Bob' to 'Fred'
var query = Query<User>(context)
  ..values.name = "Fred"
  ..where((u) => u.name).equalTo("Bob");

List<User> bobsThatAreNowFreds = await query.update();

Like  `values`,  `where`  is also the same managed object type the query is being executed on. In the above example, then, both  `values`  and  `where`  and instances of  `User`. This query executes the following SQL:

UPDATE _user SET name='Fred' WHERE name='Bob';

The  `where`  property is a very powerful and flexible, and so there is an entire section dedicated to it later in this guide. For now, we'll stick to some of the things specific to  `update()`.

Like  `insert()`, only the values set in the  `values`  property of a query get updated when executing  `update()`. Values that are omitted are not included. Values that need to be set to  `null`  must explicitly be set to  `null`  in the query:

// A Query that will remove names from anyone currently named Bob.
var query = Query<User>(context)
  ..values.name = null
  ..where((u) => u.name).equalTo("Bob");

An update query returns every modified row as a result. If no rows are updated, the return value is an empty list.

There is a variant to  `Query<T>.update`  named  `updateOne`. The  `updateOne`  method will build and execute a SQL query in the same way a normal  `update`  does, however, it will only return the single instance that was updated instead of a list. This is convenience method for the caller to get back a single instance instead of a list:

// Update user with id = 1 to have the name 'Fred'
var query = Query<User>(context)
  ..values.name = "Fred"
  ..where((u) => u.id).equalTo(1);

var updatedUser = await query.updateOne();

The  `updateOne`  method will return  `null`  if no rows were updated. It is important to note that if  `updateOne`  is used and more than one row is updated,  `updateOne`  will throw an exception and the changes to the data  _are not reversible_. Because this is likely a mistake, this is considered an error, hence the exception is thrown. It is up to the programmer to recognize whether or not a particular  `updateOne`  query would impact multiple rows.

Update queries have a safety feature that prevents you from accidentally updating every row. If you try to execute a  `Query<T>`  to do an update without configuring  `where`, an exception is thrown prior to carrying out the request. If you actually want to update every row of a table, you must set the  `Query.canModifyAllInstances`  to  `true`prior to execution. (This property defaults to  `false`.)

#### Deleting Data with a Query

A  `Query<T>`  will delete rows from a database when using  `delete()`. Like update queries, you should specify a row or rows using  `where`  properties of the  `Query<T>`. The result of a delete operation will be a  `Future<int>`  with the number of rows deleted.

var query = Query<User>(context)
  ..where((u) => u.id).equalTo(1);

int usersDeleted = await query.delete();

Also like update queries, delete queries have a safety feature that prevents you from accidentally deleting every row in a table with  `canModifyAllInstances`.

Any properties set in the query's  `values`  are ignored when executing a delete.

#### Fetching Data with a Query

Of the four basic operations of a  `Query<T>`, fetching data is the most configurable. A simple  `Query<T>`  that would fetch every instance of some entity looks like this:

var query = Query<User>(context);

List<User> allUsers = await query.fetch();

Fetch queries can be limited to a number of instances with the  `fetchLimit`  property. You may also set the  `offset`of a  `Query<T>`  to skip the first  `offset`  number of rows. Between  `fetchLimit`  and  `offset`, you can implement naive paging. However, this type of paging suffers from a number of problems and so there is another paging mechanism covered in later sections.

A fetch  `Query<T>`  uses its  `where`  property to filter the result set, just like delete and update queries. The  `values`  of a query are ignored when fetching objects. You may also fetch a single instance with  `fetchOne`. If no instance is found,  `null`  is returned. Only use this method when the search criteria is guaranteed to be unique.

var query = Query<User>(context)
  ..where((u) => u.id).equalTo(1);

User oneUser = await query.fetchOne();

When you are fetching an object by its primary key, you can use a shorthand method  `ManagedContext.fetchObjectWithID`. The method must be able to infer the type of the object, or you must provide it:

final object = await context.fetchObjectWithID<User>(1);

#### Sorting

Results of a fetch can be sorted using the  `sortBy`  method of a  `Query<T>`. Here's an example:

var q = Query<User>(context)
  ..sortBy((u) => u.dateCreated, QuerySortOrder.ascending);

`sortBy`  takes two arguments: a closure that returns which property to sort by and the order of the sort.

A  `Query<T>`  results can be sorted by multiple properties. When multiple  `sortBy`s are invoked on a  `Query<T>`, later  `sortBy`s are used to break ties in previous  `sortBy`s. For example, the following query will sort by last name, then by first name:

var q = Query<User>(context)
  ..sortBy((u) => u.lastName, QuerySortOrder.ascending)
  ..sortBy((u) => u.firstName, QuerySortOrder.ascending);

Thus, the following three names would be ordered like so: 'Sally Smith', 'John Wu', 'Sally Wu'.

#### Property Selectors

In the section on sorting, you saw the use of a  _property selector_  to select the property of the user to sort by. This syntax is used for many other query manipulations, like filtering and joining. A property selector is a closure that gives you an object of the type you are querying and must return a property of that object. The selector  `(u) => u.lastName`  in the previous section is a property selector that selects the last name of a user.

The Dart analyzer will infer that the argument of a property selector, and it is always the same type as the object being queried. This enables IDE auto-completion, static error checking, and other tools like project-wide renaming.

Live Templates

To speed up query building, create a Live Template in IntelliJ that generates a property selector when typing 'ps'. The source of the template is  `(o) => o.$END$`. A downloadable settings configuration for IntelliJ exists  [here](https://aqueduct.io/docs/intellij/)  that includes this shortcut.

### Specifying Result Properties

When executing queries that return managed objects (i.e.,  `insert()`,  `update()`  and  `fetch()`), the default properties for each object are fetched. The default properties of a managed object are properties that correspond to a database column - attributes declared in the table definition. A managed object's default properties can be modified when declaring its table definition:

class _User {
  @Column(omitByDefault: true)
  String hashedPassword;
}

Any property with  `omitByDefault`  set to true will not be fetched by default.

A property that is  `omitByDefault`  can still be fetched. Likewise, a property that is in the defaults can still be omitted. Each  `Query<T>`  has a  `returningProperties`  method to adjust which properties do get returned from the query. Its usage looks like this:

var query = Query<User>(context)
  ..returningProperties((user) => [user.id, user.name]);

`returningProperties`  is a multiple property selector - instead of returning just one property, it returns a list of properties.

You may include 'belongs-to' relationships in  `returningProperties`, but you may not include 'has-many' or 'has-one' relationships. An exception will be thrown if you attempt to. To include properties from relationships like these, see  [join in Advanced Queries](https://aqueduct.io/docs/db/advanced_queries/).

Note that if you omit the primary key of a managed object from  `returningProperties`, it will automatically be added. The primary key is necessary to transform the rows into instances of their  `ManagedObject<T>`  subclass.

#### Exceptions and Errors

When executing a query, it may fail for any number of reasons: the query is invalid, a database couldn't be reached, constraints were violated, etc. In many cases, this exception originates from the underlying database driver. When thrown in a controller, these exceptions will trigger a 500 Server Error response.

Exceptions that are thrown in response to user input (e.g., violating a database constraint, invalid data type) are re-interpreted into a  `QueryException`  or  `ValidationException`. Both of these exception types have an associated  `Response`  object that is sent instead of the default 500 Server error.

For this reason, you don't need to catch database query exceptions in a controller; an appropriate response will be sent on your behalf.

#### Statement Reuse

Aqueduct will parameterize and reuse queries when possible. This allows for significant speed and security improvements. Note that you do not have to do anything special to take advantage of this feature. However, currently at this time, you may not disable this feature.






<!--stackedit_data:
eyJoaXN0b3J5IjpbOTEyNzIxNTQ3XX0=
-->



## Security Route:


## Securing Routes with Authorizer

Instances of  `Authorizer`  are added to an application channel to verify HTTP request's authorization information before passing the request onwards. They protect channel access and typically come right after  `route`. Here's an example:

```dart
@override
Controller get entryPoint {
  final router = Router();

  router
    .route("/protected")
    .link(() => Authorizer.bearer(authServer))
    .link(() => ProtectedController());

  router
    .route("/other")
    .link(() => Authorizer.basic(authServer))
    .link(() => OtherProtectedController());

  return router;
}
```

An  `Authorizer`  parses the Authorization header of an HTTP request. The named constructors of  `Authorizer`indicate the required format of Authorization header. 

- The  **`Authorization.bearer()`** 
   constructor expects an OAuth 2.0 bearer token in the header, which has the following format:

	- Authorization: **Bearer 768iuzjkx82jkasjkd9z9**

>  ##### 📝 **bear** - a word borrowed from client-certificate authenticcateion
>  Client-certificate authentication is a more secure method of authentication than either basic or form-based authentication. It **uses HTTP over SSL**, in which the server and, optionally, the client **authenticate one another** using public key certificates.  Secure Socket Layer  (SSL) provides data encryption, server authentication, message integrity, and optional client authentication for a TCP/IP connection. **You can think of a  public key certificate  as the digital equivalent of a passport**. It is issued by a trusted organization, which is called a  certificate authority  (CA), and provides identification for the **bearer**.

- **`Authorizer.basic`**  
  expects HTTP Basic Authentication, where the username and password are joined with the colon character (`:`) and Base 64-encoded:

	- // 'dXNlcjpwYXNzd29yZA==' is 'user:password'
	Authorization: **Basic dXNlcjpwYXNzd29yZA==**


If the header can't be parsed, doesn't exist or is in the wrong format, an  `Authorizer`  responds to the request with a 401 status code and prevents the next controller from receiving the request.

> ⚡ Once parsed, an  `Authorizer`  sends the information - either the bearer token, or the username and password - to its  `AuthServer`  for verification. If the  `AuthServer`  rejects the authorization info, the  `Authorizer`  responds to the request with a 401 status code and prevents the next controller from receiving the request. Otherwise, the request continues to the next controller.

- For  `Authorizer.bearer`
the value in a request's header must be a valid, **unexpired access token**. These types of authorizers are used when an endpoint requires a **logged in user**.

- For  `Authorizer.basic`  
authorizers, credentials are verified by finding an OAuth 2.0 client identifier and ensuring its client secret matches. Routes with this type of authorizer are known as  **_client authenticated_**  routes. These types of authorizers are used when an endpoint requires a **valid client application**, but **not** a logged in user.

#### Authorizer and OAuth 2.0 Scope

An  `Authorizer`  may restrict access to controllers based on the scope of the request's bearer token. By default, an  `Authorizer.bearer`  allows any valid bearer token to pass through it. If desired, an  `Authorizer`  is initialized with a list of required scopes. A request may only pass the  `Authorizer`  if it has access to  _all_  scopes listed in the  `Authorizer`. For example, the following requires at least  `user:posts`  and  `location`  scope:

```dart
router
  .route("/checkin")
  .link(() => Authorizer.bearer(authServer, scopes: ["user:posts", "location"]))
  .link(() => CheckInController());
```

Note that you don't have to use an  `Authorizer`  to restrict access based on scope. A controller has access to scope information after the request has passed through an  `Authorizer`, so it can use the scope to make more granular authorization decisions.

#### Authorization Objects

For example, a social networking application might have a  `/news_feed`  endpoint protected by an  `Authorizer`. When an authenticated user makes a request for  `/news_feed`, the controller will return that user's news feed. It can determine this by using the  `Authorization`:

```dart
class NewsFeedController extends ResourceController {
  NewsFeedController(this.context);
  ManagedContext context;

  @Operation.get()
  Future<Response> getNewsFeed() async {
    var forUserID = request.authorization.ownerID;

    var query = Query<Post>(context)
      ..where((p) => p.author).identifiedBy(forUserID);

    return Response.ok(await query.fetch());
  }
}
```
In the above controller, it's impossible for a user to access another user's posts.

`Authorization`  objects also retain the scope of an access token so that a controller can make more granular decisions about the information/action in the endpoint. Checking whether an  `Authorization`  has access to a particular scope is accomplished by either looking at the list of its  `scopes`  or using  `authorizedForScope`:

```dart
class NewsFeedController extends ResourceController {
  NewsFeedController(this.context);
  ManagedContext context;

  @Operation.get()
  Future<Response> getNewsFeed() async {
    if (!request.authorization.authorizedForScope("user:feed")) {
      return Response.unauthorized();
    }
    var forUserID = request.authorization.ownerID;
    var query = Query<Post>(context)
      ..where((p) => p.author).identifiedBy(forUserID);

    return Response.ok(await query.fetch());
  }
}
```

#### Using Authorizers Without AuthServer

Throughout this guide, the argument to an instance of  `Authorizer`  has been referred to as an  `AuthServer`. This is true - but only because  `AuthServer`  implements  `AuthValidator`.  `AuthValidator`  is an interface for verifying bearer tokens and username/password credentials.

You may use  `Authorizer`  without using  `AuthServer`. For example, an application that **doesn't** use OAuth 2.0 could provide its own  `AuthValidator`  interface to simply verify the username and password of every request:

```dart
class BasicValidator implements AuthValidator {
  @override
  FutureOr<Authorization> validate<T>(AuthorizationParser<T> parser, T authorizationData, {List<AuthScope> requiredScope}) {}
    var user = await userForName(usernameAndPassword.username);
    if (user.password == hash(usernameAndPassword.password, user.salt)) {
      return Authorization(...);
    }

    // Will end up creating a 401 Not Authorized Response
    return null;
  }
}
```

- The  `validate`  method must return an  
	- **`Authorization`**  if the credentials are **valid**, or 
	- **null** if they are **not**. 

The  `parser`  lets the validator know the format of the Authorization header (e.g., 'Basic' or 'Bearer') and  `authorizationData`  is the meaningful information in that header. 

There are two concrete types of  `AuthorizationParser<T>`
- `AuthorizationBasicParser` : 
	The authorization data for a basic parser is an instance of  `AuthBasicCredentials`  that contain the username and password 
- `AuthorizationBearerParser`: 
	the bearer parser's authorization data is the bearer token string.



























<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEzODExODg1NF19
-->

## Setup Auth:
## Creating AuthServers to Authenticate and Authorize

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



### Creating Instances of AuthServer and AuthServerDelegate

`AuthServerDelegate`  is an interface that an  `AuthServer`  uses to handle storage of client identifiers, tokens and other authorization artifacts. An  `AuthServer`  must be created with a concrete implementation of  `AuthServerDelegate`. Aqueduct contains a concrete implementation of  `AuthServerDelegate`  that uses the ORM. It is highly recommended to use this implementation instead of implementing your own storage because it has been thoroughly tested and handles cleaning up expired data correctly.

This concrete implementation is named  `ManagedAuthDelegate<T>`. It exists in a sub-package of Aqueduct and must be explicitly imported. Here's an example of creating an  `AuthServer`  and  `ManagedAuthDelegate<T>`:

```dart
import 'package:aqueduct/aqueduct.dart';
import 'package:aqueduct/managed_auth.dart';

class MyApplicationChannel extends ApplicationChannel {  
  AuthServer authServer;
  @override
  Future prepare() async {
    final context = ManagedContext(...);
    final delegate = ManagedAuthDelegate<User>(context);
    authServer = AuthServer(delegate);
  }
  ...
}
```
While `AuthServer` has methods for handling authorization tasks, it is rarely used directly. Instead, `AuthCodeController` and `AuthController` are hooked up to routes to grant authorization tokens through your application's HTTP API. Instances of `Authorizer` secure routes in channels. All of these types invoke the appropriate methods on the `AuthServer`. Here's an example `ApplicationChannel` subclass that sets up and uses authorization:

```dart
import 'package:aqueduct/aqueduct.dart';
import 'package:aqueduct/managed_auth.dart';

class MyApplicationChannel extends ApplicationChannel {
  AuthServer authServer;
  ManagedContext context;
  @override
  Future prepare() async {
    context = ManagedContext(...);
    final delegate = ManagedAuthDelegate<User>(context);
    authServer = AuthServer(delegate);
  }
  @override
  Controller get entryPoint {
    final router = Router();
    // Set up auth token route- this grants and refresh tokens
    router.route("/auth/token").link(() => AuthController(authServer));
    // Set up auth code route- this grants temporary access codes that can be exchanged for token
    router.route("/auth/code").link(() => AuthCodeController(authServer));
    // Set up protected route
    router
      .route("/protected")
      .link(() => Authorizer.bearer(authServer))
      .link(() => ProtectedController());
    return router;
  }
}
```
### Using ManagedAuthDelegate

`ManagedAuthDelegate<T>`  is a concrete implementation of  `AuthServerDelegate`, providing storage of authorization tokens and clients for an  `AuthServer`. Storage is accomplished by Aqueduct's ORM.  `ManagedAuthDelegate<T>`, by default, is not part of the standard  `aqueduct`  library. To use this class, an application must import  `package:aqueduct/managed_auth.dart`.

The type argument to  `ManagedAuthDelegate<T>`  represents the application's concept of a 'user' or 'account' - OAuth 2.0 terminology would refer to this type as a  **_resource owner_**. 

A resource owner must be a  `ManagedObject<T>`  subclass that is specific to your application. Its **table** definition  **_must extend_** `ResourceOwnerTableDefinition`  and the instance type must implement  `ManagedAuthResourceOwner<T>`, where  `T`  is the table definition. A basic definition may look like this:

```dart
class User extends ManagedObject<_User>
    implements _User, ManagedAuthResourceOwner<_User> {
}
class _User extends ResourceOwnerTableDefinition {
  @Column(unique: true)
  String email;
}
```

By extending  **`ResourceOwnerTableDefinition`**  in the table definition, the database table has the following four columns:

-   an integer primary key named  `id`
-   a unique string  `username`
-   a password hash
-   a salt used to generate the password hash

A  `ResourceOwnerTableDefinition`  also has a  `ManagedSet`  of  `tokens`  for each token that has been granted on its behalf.

The interface  `ManagedAuthResourceOwner<T>`  is a requirement that ensures the type argument is both a  `ManagedObject<T>`  and  `ResourceOwnerTableDefinition`, and serves no other purpose than to restrict  `ManagedAuthDelegate<T>`'s type parameter.

This structure allows an application to declare its own 'user' type while still enforcing the needs of Aqueduct's OAuth 2.0 implementation.

The  `managed_auth`  library also declares two  `ManagedObject<T>`  subclasses.  `ManagedAuthToken`  represents instances of authorization tokens and codes, and  `ManagedAuthClient`  represents instances of OAuth 2.0 clients. This means that an Aqueduct application that uses  `ManagedAuthDelegate<T>`  has a minimum of three database tables: users, tokens and clients.

`ManagedAuthDelegate<T>`  will delete authorization tokens and codes when they are no longer in use. This is determined by how many tokens a resource owner has and the tokens expiration dates. Once a resource owner acquires more than 40 tokens/codes, the oldest tokens/codes (determined by expiration date) are deleted. Effectively, the resource owner is limited to 40 tokens. This number can be changed when instantiating  `ManagedAuthDelegate<T>`:


















































<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzNTMyMTkzLDEyMzM2MjI4NTksMTU4Mj
k5Mzg0MSwtNDE2OTQwMDk1LC0xNzkzNDQyNTEwLDE4MDQ4OTg1
OCwtMjcxNDg0MDkyLDE3NjA0MjI5Ml19
-->

## Understand Authentication:

following material fetched from [here](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html)

 ### Understanding Login Authentication

When you try to access a protected web resource, the web container activates the authentication mechanism that has been configured for that resource. You can specify the following authentication mechanisms:

-   HTTP basic authentication
-   Form-based login authentication
-   Client certificate authentication
-   Mutual authentication
-   Digest authentication

If you do not specify one of these mechanisms, the user will not be authenticated.

#### Using HTTP Basic Authentication

[Figure 32-2](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483341)  shows what happens if you specify  _HTTP basic authentication_.

![HTTP Basic Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-httpBasicAuthentication.gif)

Figure 32-2 HTTP Basic Authentication

With basic authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server returns a dialog box that requests the user name and password.
-   The client submits the user name and password to the server.
-   The server validates the credentials and, if successful, returns the requested resource.

HTTP basic authentication is **not** particularly secure. Basic authentication sends user names and passwords over the Internet as text that is uu-encoded (Unix-to-Unix encoded) but not encrypted. This form of authentication, which **uses Base64 encoding**, can expose your user names and passwords **unless all connections are over SSL**. If someone can intercept the transmission, the user name and password information can easily be decoded.

[Example: Basic Authentication with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp156943)  is an example application that uses HTTP basic authentication in a JAX-RPC service.

#### Using Form-Based Authentication

[Figure 32-3](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483393)  shows what happens if you specify  _form-based authentication_, in which you can customize the login screen and error pages that an HTTP browser presents to the end user.

![Form-Based Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-formBasedLogin.gif)

Figure 32-3 Form-Based Authentication

With form-based authentication, the following things occur:

-   A client requests access to a protected resource.
-   If the client is unauthenticated, the server redirects the client to a login page.
-   The client submits the login form to the server.
-   If the login succeeds, the server redirects the client to the resource. If the login fails, the client is redirected to an error page.

Form-based authentication is **not** particularly secure. In form-based authentication, the content of the user dialog box is sent as plain text, and the target server is not authenticated. This form of authentication can expose your user names and passwords **unless all connections are over SSL**. If someone can intercept the transmission, the user name and password information can easily be decoded.

[Example: Using Form-Based Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp303355)  is an example application that uses form-based authentication.



#### Using Client-Certificate Authentication

Client-certificate authentication is a more secure method of authentication than either basic or form-based authentication. It **uses HTTP over SSL**, in which the server and, optionally, the client **authenticate one another** using public key certificates.  Secure Socket Layer  (SSL) provides data encryption, server authentication, message integrity, and optional client authentication for a TCP/IP connection. **You can think of a  public key certificate  as the digital equivalent of a passport**. It is issued by a trusted organization, which is called a  certificate authority  (CA), and provides identification for the **bearer**.

If you specify  client-certificate authentication, the web server will authenticate the client using the client's  X.509 certificate, a public key certificate that conforms to a standard that is defined by X.509 Public Key Infrastructure (PKI). Before running an application that uses SSL, you must configure SSL support on the server (see  [Installing and Configuring SSL Support](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security6.html#wp509934)) and set up the public key certificate (see  [Understanding Digital Certificates](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security6.html#wp80737)).

[Example: Client-Certificate Authentication over HTTP/SSL with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp498398)  describes an example application that uses client-certificate authentication.



#### 📝 Using Mutual Authentication

With  _mutual authentication_ , the server and the client authenticate each other. There are two types of mutual authentication:

-   **Certificate-based** mutual authentication (see  [Figure 32-4](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp488092))
-   **User name- and password-based** mutual authentication (see  [Figure 32-5](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483607))


[Figure 32-4](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp488092)  shows what occurs during certificate-based mutual authentication.

![Certificate-Based Mutual Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-sslBasedMutualAuthenticationWithCertificates.gif)

⚡ Figure 32-4 **Certificate-Based** Mutual Authentication

In certificate-based mutual authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server presents its certificate to the client.
-   The client verifies the server's certificate.
-   If successful, the client **sends its certificate** to the server.
-   The server verifies the client's credentials.
-   If successful, the server grants access to the protected resource requested by the client.

[Example: Client-Certificate Authentication over HTTP/SSL with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp498398)  describes an example application that uses certificate-based mutual authentication.

[Figure 32-5](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483607)  shows what occurs during user name- and password-based mutual authentication.

![Username/Password-Based Mutual Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-sslBasedMutualAuthenticationWithUsernamePassword.gif)

⚡ Figure 32-5 User Name- and Password-Based Mutual Authentication

In user name- and password-based mutual authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server presents its certificate to the client.
-   The client verifies the server's certificate.
-   If successful, the client sends **its user name and password** to the server, which verifies the client's credentials.
-   If the verification is successful, the server grants access to the protected resource requested by the client.



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI2MTcyMTg0M119
-->

## Codecs:

Not all content types require two conversion steps. For example, when serving an HTML file, the body object is already an HTML  `String`. It will only be converted by a charset encoder:
```dart
var html = "<html></html>";
var response = Response.ok(html)
  ..contentType = ContentType.html;
```
#### Image Codec
An image body object needs no conversion at all, since it is already a list of bytes. **If** there is **no registered codec** for a content-type, the body object **must be a byte array** (`List<int>`  where each value is between 0-255).
```dart
final imageFile = File("image.jpg");
final imageBytes = await imageFile.readAsBytes();
final response = Response.ok(imageBytes)
  ..contentType = ContentType("image", "jpeg");
```

##### disable automatic encoding

You may **disable** the **automatic encoding** of a body as long as the body object is a **byte** array:
```dart
final jsonBytes = utf8.encode(json.encode({"key": "value"}));
final response = Response.ok(jsonBytes)..encodeBody = false;
```
See a later section for more details on content type to codec mappings. Also, see the documentation for  `CodecRegistry`  for details on built-in codecs and adding codecs.


### Streaming Response Bodies

A **body** object **may also be a  `Stream<T>`**.  `Stream<T>`  body objects are most **often used when serving files**. This allows the contents of the file to be **streamed from disk** to the HTTP client **without** having to load the whole file into memory first. (See also  `FileController`.)
```dart
final imageFile = File("image.jpg");
final imageByteStream = imageFile.openRead();
final response = new Response.ok(imageByteStream)
  ..contentType = new ContentType("image", "jpeg");
```
> ⚡ When a body object is a  `Stream<T>`, the response will not be sent until the stream is closed. 

> For finite streams - like those from opened filed - this happens as soon as the entire file is read. For streams that you construct yourself, you must close the stream some time after the response has been returned.

## Codecs and Content Types

In the above sections, we glossed over how a codec gets selected when preparing the response body. The common case of  `ManagedObject<T>`  body objects that are sent as UTF8 encoded JSON 'just works' and is suitable for most applications. When serving assets for a web application or different data formats like XML, it becomes important to understand how Aqueduct's codec registry works.

> ⚡ `CodecRegistry`  contains mappings from content types to  `Codec`s. These codecs **encode response** bodies and **decode request** bodies. 
There are three built-in codecs for 
  - `application/json`, 
  - `application/x-www-form-urlencoded`  and  
  - `text/*`. 

> ⚡ When a response is being sent, the repository is searched for an entry that exactly matches the primary and subtype of the  `Response.contentType`. If an entry exists, the associated  `Codec`  starts the conversion. 

For example, if the content type is  `application/json; charset=utf-8`, the built-in  `application/json`  codec encodes the body object.

> ⚡ If there isn't an exact match, but there is an entry for the primary type with the wildcard (`*`) subtype, that codec is used. For example, the built-in codec for  `text/*`will be selected for both  `text/plain`  and  `text/html`. 

### register codecs into CodeRegistery

If there was something special that had to be done for  `text/html`, a more specific codec may be added for that type:
```dart
class MyChannel extends ApplicationChannel {
  @override
  Future prepare() async {
    CodecRegistry.defaultInstance.add(ContentType("application", "html"), HTMLCodec());
  }
}
```
> ⚡ Codecs must be added in your  `ApplicationChannel.prepare`  method. **The codec must implement  `Codec`  from  `dart:convert`**. In the above example, when a response's content type is  `text/html`, the  `HTMLCodec`  will encode the body object. This codec takes precedence over  `text/*`  because it is more specific.

> 📓 When selecting a codec for a response body, the  `ContentType.charset`  doesn't impact which codec is selected. 

If a response's content-type has a charset, then a charset encoder like  `UTF8`  will be applied as a last encoding step. For example, a response with content-type  `application/json; charset=utf-8`  will encode the body object as a JSON string, which is then encoded as a list of UTF8 bytes. It is required that a response body's eventually encoded type is a list of bytes, so it follows that a codec that produces a string must have a charset.

> ⚡ If there is no codec in the repository for the content type of a  `Response`, the body object must be a  `List<int>`  or  `Stream<List<int>>`. 
>  📘 If you find yourself converting data prior to setting it as a body object, it may make sense to add your own codec to  `CodecRegistry`.

> ⚡ A request's body always starts as a **list of bytes** and is decoded into Dart objects. To decode a JSON request body, it first must be decoded from the **list of UTF8** bytes into a **string**. 

#### register JsonCodec into CodecRegistry

It is possible that a client could omit the charset in its content-type header. Codecs added to  `CodecRegistry`  may specify a default charset to interpret a charset-less content-type. When a codec is added to the repository, if content-type's charset is non-null, that is the default. For example, the JSON codec is added like this:
```dart
CodecRegistry.defaultInstance.add(
  ContentType("application", "json", charset: "utf-8"),
  const JsonCodec(),
  allowCompression: true);
```
If no charset is specified when registering a codec, no charset decoding occurs on a request body if one  doesn't exist. Content-types that are decoded from a  `String`should not use a default charset because the repository would always attempt to decode the body as a string first.

### Compression with gzip

Body objects may be compressed with  `gzip`  if the HTTP client allows it  _and_  the  `CodecRegistry`  has been configured to compress the content type of the response. 

> ⚡ The three built-in codecs 
> -  `application/json`
> -   `application/x-www-form-urlencoded`  and  
> - `text/*`  
are all configured to allow compression. Compression occurs as the last step of conversion and only if the HTTP client sends the  `Accept-Encoding: gzip`  header.

Content types that are not in the codec repository will not trigger compression, even if the HTTP client allows compression with the  `Accept-Encoding`  header. This is to prevent binary contents like images from being 'compressed', since they are likely already compressed by a content-specific algorithm. 

> ⚡ In order for Aqueduct to compress a content type other than the built-in types, you may add a codec to the repository with the  `allowCompression`  flag. (The default value is  `true`.)
```dart
CodecRegistry.add(
  ContentType("application", "x-special"),
   MyCodec(),
  allowCompression: true);
```
You may also set whether or not a content type uses compression without having to specify a codec if no conversion step needs to occur:
```dart
CodecRegistry.setAllowsCompression(new ContentType("application", "x-special"), true);
```

## Serializable Objects

Encoding and decoding primarily takes or yields simple Dart objects like  `Map`,  `List`and  `String`. It is often beneficial to add structure to body objects for use in your code. Classes that implement  `Serializable`  provide this structure. A  `Serializable`  object must implement a  `readFromMap()`  and  `asMap()`  method to convert their structure into or from a Dart  `Map`.

> ⚡ An object that extends  `Serializable`  may be used as a response body object directly:

```dart
class Person extends Serializable {
  String name;
  String email;
  Map<String, dynamic> asMap() {
    return {
      "name": name,
      "email": email
    };
  }
  void readFromMap(Map<String, dynamic> inputMap) {
    name = inputMap['name'];
    email = inputMap['email'];
  }
}

final person = Person();
final response = Response.ok(person);
```

> ⚡ When responding with a  `Serializable`, its  **`asMap()`**  is called **prior** to any **encoding** by the codec registry.  `ManagedObject<T>`, part of the Aqueduct ORM, implements  `Serializable`  so results from  `Query<T>`  may be body objects:
```dart
final query = Query<Person>(context)..where((p) => p.id).equalTo(1);
final person = await query.fetchOne();
final response = Response.ok(person);
```
A response body object can also be a list of  `Serializable`  objects.
```dart
final query = Query<Person>(context);
final people = await query.fetch();
final response = Response.ok(people);
```
The entire flow of a body object is shown in the following diagram. Each orange item is an allowed body object type and shows the steps it will go through when being encoded to the HTTP response body. For example, a  `Serializable`  goes through three steps, whereas a  `List<int>`  goes through zero steps and is added as-is to the HTTP response.

![Response Body Object Flow](https://aqueduct.io/docs/img/response_flow.png)

A serializable object can be read from a request body:
```dart
final person = Person()..readFromMap(await request.body.decode());
```

> ⚡ **Serializable** objects are the only types of objects that can be  [bound to a ResourceController argument](https://aqueduct.io/docs/http/resource_controller/).

```dart
@Operation.post()
Future<Response> addPerson(@Bind.body() Person person) async {
  final insertedPerson = await context.insertObject(person);
  return Response.ok(insertedPerson);
}
```
### Serializable and OpenAPI Generation

See the section on how  `Serializable`  types work with OpenAPI documentation generation  [here](https://aqueduct.io/docs/openapi/components/).

## 🔥 Example: multipart/form-data
**Add  `package:mime`  to your  `pubspec.yaml`  as a dependency.**
```
dependencies:
  mime: any # prefer a better constraint than this
```

And then decode the body with the objects from that package:

```dart
import 'package:aqueduct/aqueduct.dart';
import 'package:mime/mime.dart';

class MyController extends ResourceController {
  MyController() {
    acceptedContentTypes = [ContentType("multipart", "form-data")];
  }

  @Operation.post()
  Future<Response> postForm() async {}
    final transformer = MimeMultipartTransformer(request.raw.headers.contentType.parameters["boundary"]);
    final bodyStream = Stream.fromIterable([await request.body.decode<List<int>>()]);
    final parts = await transformer.bind(bodyStream).toList();

    for (var part in parts) {
      final headers = part.headers;
      final content = await part.toList();
      ...
    }    
  }
}
```
> 📘 Further explanation
>  **MimeMultipartTransformer**
> >Parser for MIME multipart types of data as described in RFC 2046 section 5.1.1. The data is transformed into [MimeMultipart](https://pub.dartlang.org/documentation/mime/latest/mime/MimeMultipart-class.html) objects, each of them streaming the multipart data.
	
> **methods**
> **bind**(**Stream<List<int>>** stream) → **Stream**<**MimeMultipart**>
> 
> > Transforms the provided  `stream`. Returns a new stream with events that are computed from events of the provided  `stream`.



## Processing MIME multipart media types  [#](https://pub.dartlang.org/packages/mime#processing-mime-multipart-media-types)

The class  **`MimeMultipartTransformer`**  is used to process a  **`Stream`**  of bytes encoded using a **MIME multipart media types encoding**. The transformer provides a new  `Stream`  of  `MimeMultipart`  objects each of which have the headers and the content of each part. The content of a part is provided as a stream of bytes.

> 📘 上述 The transformer provides a new  `Stream` .... and the content of each part. 指的是multipart header 中每個被boundary分割的區塊

Below is an example showing how to process an HTTP request and print the length of the content of each part.
```dart
// HTTP request with content type multipart/form-data.
HttpRequest request = ...;
// Determine the boundary form the content type header
String boundary = request.headers.contentType.parameters['boundary'];
// Process the body just calculating the length of each part.
request
    .transform(new MimeMultipartTransformer(boundary))
    .map((part) => part.fold(0, (p, d) => p + d))
    .listen((length) => print('Part with length $length'));
```



> #### How to Send Form Data in Dart
> 
```dart
		FormData data = new FormData(); // from dart:html
		data.append(key, value);
		HttpRequest.request('/upload', method: 'POST', sendData: data).then((HttpRequest r) 
```

> #### Send Streaming Data via FormData by powerful Http client, dio
> 
```dart
FormData formData = new FormData.from({
    "name": "wendux",
    "age": 25,
    "file": new UploadFileInfo(new File("./example/upload.txt"), "upload.txt")
});
response = await dio.post("/info", data: formData);
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyMjAwNjQxMSw3NzMzMjk0NTYsLTEwOT
Y3MTI1MDUsNTQwNjYzNDg1LDEyNDY2MDczNCwtMTExNDg3MTY3
LDE3NTgwNDAwNTgsLTUwNTMxNDEwMF19
-->

## Core Concept:

## Core Concepts

### Resources
Resources are the things your application exposes through its HTTP API.
In HTTP, resource means data which is the one being transferred and manipulated.

-   GET — Get a list of resources or single resource
-   POST — Create new resource
-   PUT — Update existing resource
-   DELETE — Delete a resource

### Routing
The route /organizations/:id will match the paths /organizations/1, /organizations/2, and so on.



### Controllers
Controllers are objects that handle requests. For example, a controller might fetch rows from a database and send them to the client in the response body. Another controller might verify the username and password of a request's Authorization header are valid. **Controllers are linked together to form a series of actions to take for a request. These linked together controllers are called a _channel_.**

There are two flavors of controllers. 
- #### _endpoint controller_  
	 performs operations on a resource or resource collection, and always sends a response. Endpoint controllers  _fulfill_  requests by returning the state of a resource or by changing the state of a resource. You write most of your application-specific logic in endpoint controllers.

- #### _middleware controller_  
	takes an action for a request, but isn't responsible for fulfilling the request. Middleware controllers can do many different things and are often reusable in many channels. Most often, a middleware controller validates something about a request before it reaches an endpoint controller. Middleware controllers can send a response for a request, and doing so prevents any other controller in that channel from handling the request.

A channel must have exactly one endpoint controller. It can be preceded by zero or more middleware controllers. See the guides on  [Controllers](https://aqueduct.io/docs/http/controller/)  and  [ResourceControllers](https://aqueduct.io/docs/http/resource_controller/)  for usage details.



#### @Operation.get 

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

### Request Binding @bind

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


### Using ManagedContext to Connect to a Database


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


### Configuration Files

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

 

#### Configuration Conventions and Deployment Options

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
eyJoaXN0b3J5IjpbLTEwNzM4MDc3NDAsLTgwNTIxOTg0NiwzNT
g5OTY5MzEsLTE2OTY5MzM4MTEsMTU2MjU0NTA0MSwxMjQxMTE2
Mjg1LC00MTA0ODA0OSwtMzE4MTE1NjYxLDQ3Mjk5OTQxMSw2Mj
AwODIyMzgsLTg1MjA1NjU0NCwtNjcxNjkzMzIzLC0yMDE2NzEz
NDAxLC00NDM2NDY3OCwtMTE4OTg0OTA1LDUzODkyNDcxMiwxNz
kzMjQyODU3LC01Njg1Mzk4NSwyMDg2MDA4ODA0LDE1NTkzMTEy
NDFdfQ==
-->

## DataBase:



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

> #### ⚡ @Relate 關連欄位
> 需要在table 插入時一併寫入該欄位，否則會出現錯誤
> #### ⚡ ManagedSet<ReferencedType>
> 此為Query指向用,無實際欄位, 不得插入 


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

By default, transient properties - those declared in the **managed** object subclass, **not the table definition** - are  **_not_**  included in an object's  `asMap()`. The  **`Serialize`**  annotation **allows** a transient property to be included in this map.

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
> ⚡ Getters with the `Serialize` annotation will be written in `asMap` and setters with the annotation will be read in `readFromMap`.

> ⚡ You can annotate a transient property with  `Serialize`   so that it is able to be **read from a request body**, **written to a response body**, or both. For example:

```dart
class Employee extends ManagedObject<_Employee> implements _Employee {
  int a; // NOT included in asMap, NOT read in readFromMap
  @Serialize() int b; // included in asMap, read in readFromMap
  @Serialize(input: true, output: false) int c; // NOT included in asMap, read in readFromMap
  @Serialize(input: false, output: true) int d; // included in asMap, NOT read in readFromMap
}
class _Employee {
  @primaryKey
  int id;
}
```

 
## Auth
 
```dart
class _User extends ResourceOwnerTableDefinition {  
  @Column(unique: true)  
  String email;  
  ManagedSet<Note> notes;  
}
class User extends ManagedObject<_User> implements _User, ManagedAuthResourceOwner<_User> {  
  @Serialize(input: true, output: false)  
  String password;
}
class RegisterController extends QueryController<User> {  
  RegisterController(ManagedContext context, this.authServer) : super(context);  
  final AuthServer authServer;  
  
  @Operation.post()  
  Future<Response> createUser() async {  
    if (query.values.username == null || query.values.password == null) {  
      return Response.badRequest(body: {"error": "username and password required."});  
  }  
  
  query.values.username = query.values.username.toLowerCase();  
  
  final salt = AuthUtility.generateRandomSalt();  
  final hashedPassword = AuthUtility.generatePasswordHash(query.values.password, salt);  
  
  query.values.hashedPassword = hashedPassword;  
  query.values.salt = salt;  
  query.values.email = query.values.username;  
  
  final u = await query.insert();  
  final token = await authServer.authenticate(  
      u.username,  
  query.values.password,  
  request.authorization.credentials.username,  
  request.authorization.credentials.password);  
  
  return AuthController.tokenResponse(token);  
  }  
}
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTkzMjgzNTcsMTUyNDg4MjU5MSwtMT
c2MDYyNDE0MywtMTIyNjAwOTU1NywtMTUzNDcyMDA4NSw4NDE3
OTQzMTQsLTEyODc0NjA5ODAsOTM1MTY1NzY3LDE5NTQ5Nzg4NT
csMzQ0MDUxODk5LDY4MzMyMjg3MiwtMTcwMTE0MjU5NCwtMTAz
NTMxNjY5MiwyMDcwMjI3NDA4LDEzNzI3NTg2OTYsMTI0NDA1NT
gxLDIwODk3MjY0MzRdfQ==
-->

## JSON:
## JSON Document Storage

Learn how to store unstructured, binary JSON data in  `ManagedObject<T>`properties.

### JSON Columns in Relational Databases

PostgreSQL supports many column data types like integers, strings, booleans and dates. A column may also be JSON data. This allows for **storing unstructured data** and **simple objects** in a table column. The data from JSON columns can be fetched all at once, or in pieces. Elements of JSN data can be used to filter the results of a query.

### The Document Data Type

JSON document columns are added to a database table by declaring a  **`Document`property** in a  `ManagedObject<T>`'s table definition. In PostgreSQL, a  **`Document`**column data type is  **`jsonb`**. A document column can only contain **JSON-encodable** data. This data is typically a  **`Map`**  or  **`List`**  that contains only **JSON-encodable** data. The following  `ManagedObject<T>`  declaration will have a  `contents`  column of type  `jsonb`.
```dart
class Event extends ManagedObject<_Event> implements _Event {}
class _Event {
  @primaryKey
  int id;
  @Column(indexed: true)
  DateTime timestamp;
  Document contents;
}
```

> ⚡ A  **`Document`**  object has a  **`data`**  property to hold its **JSON-encodable** data. When instantiating  `Document`, this property **defaults to null unless** a value has been provided to the optional, ordered parameter in its constructor.
```dart
final doc = new Document();
assert(doc.data == null);

final doc = new Document({"key": "value"});
assert(doc.data is Map);

final doc = new Document([0]);
assert(doc.data is List);
```
The data in a document can be accessed through its  `data`  property, or through its subscript operator.  `Document`'s subscript operator forwards the invocation to its  `data`  property.
```dart
final doc = new Document({"key": "value"});
assert(doc["key"] == doc.data["key"]);
```
The argument to the subscript operator may be a string (if  `data`  is a map) or an integer (if  `data`  is a list).

### Basic Operations on Document Properties

`Document`  columns are like any other type of column, and can therefore be set during an insert or update, and read during a fetch.

#### Inserting Rows with Document Properties

A  **`Document`**  property is first set **when inserting** with a  `Query<T>`. The  `values`property of the query is set to a  `Document`  object initialized with a JSON-encodable value.
```dart
final query = Query<Event>(context)
  ..values.timestamp = DateTime.now()
  ..values.contents = Document({
    "type": "push",
    "user": "bob",
    "tags": ["v1"]
  });
final event = await query.insert();  
```
In the above, the argument to  `Document`  will be JSON-encoded and stored in the database for column  `contents`. If the object can't be encoded as JSON, an exception will be thrown.

#### Fetching Rows with Document Properties

> ⚡ When fetching an object with  `Document`  properties with a  `Query<T>`, you **access the column's value through the document's  `data`**  property.
```dart
final query = Query<Event>(context)
  ..where((e) => e.id).equalTo(1);
final event1 = await query.fetchOne();
event1.contents.data == {
  "type": "push",
  "user": "bob",
  "tags": ["v1"]
};
```
When fetching  `Document`  properties, the JSON data is decoded into the appropriate type. This is likely a  `Map`  or  `List`, but can be any JSON-encodable object. 

> ⚡ Because the data stored in a  `Document`  property is unstructured, the type of  `data`  is  `dynamic`. It is good practice to **store consistent data structures** in a column; i.e., always storing a  `Map`  or always storing a  `List`.

#### Updating Rows with Document Properties

Updating a row with  `Document`  properties works the same as inserting rows.
```dart
final query = Query<Event>(context)
  ..where((e) => e.id).equalTo(1)
  ..values.contents = Document({
    "type": "push",
    "user": "bob",
    "tags": ["v1", "new"]
  });
final event = await query.updateOne();  
```
When updating in this way, the document stored in the column is replaced entirely.

#### Accessing Document Values

**The type of  `Document.data`  is  `dynamic**`  - it can be any valid JSON type and may be casted to the expected type when used. This data can also be nested - a  `List`of  `Maps`, for example. When accessing object keys or list indices, you may use the subscript operator directly on  `Document`.

```dart
// Object Access by key
final doc = Document({"key": "value"});
final value = doc["key"] == "value";
// List Access by index
final doc = Document(["v1", "v2"]);
final value = doc[0] == "v1";
```
You can access nested elements with the same syntax:

```dart
final doc = Document([
  {"id": 1},
  {"id": 2}
]);
final obj1 = doc[0]["id"]; // == 1
final obj2 = doc[1]["id"]; // == 2
```
> ⚡ Note that **using the subscript operator on a  `Document`  simply invokes it on its  `data`  property**. Therefore, any subscript values must be valid for Dart  `List`  and  `Map`  types.

### Fetching Sub-documents

> ⚡ When fetching a  `Document`  property, the default behavior is to return the entire JSON document as it is stored in the database column. **You may fetch parts of the document you need** by using  **`Query.returningProperties`**  and the subscript operator.
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"]]);
final eventsWithTags = query.fetch();
```
When using the subscript operator on a returned  `Document`  property, only the value for that key is returned. For example, if the above query were executed and the stored column's value were:
```json
{
  "type": "push",  
  "user": "bob",
  "tags": ["v1"]  
}
```
The value of  `Event.contents`  would only contain the array for the key "tags":
```json
["v1"]
```
You may also index arrays in a JSON column using the same subscript operator, and the subscript operator can also be nested. For example, the following query would fetch the "tags" array, and then fetch the string at index 0 from it:
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][0]]);
final eventsWithFirstTag = await query.fetchOne();
eventsWithFirstTag.contents.data == "v1";
```
If a key or index does not exist in the JSON document, the value of the returned property will be null. For this reason, you should use null-aware operators when accessing  `Document.data`:
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][7]]); // 7 is out of bounds
final eventsWithFirstTag = await query.fetchOne();
if (eventsWithFirstTag.contents?.data == "v1") {
  ...
}
```
When fetching elements from a JSON array, you may use negative indices to specify a index from the end of the array.
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][-1]]);
final eventsWithLastTag = await query.fetchOne();
```
Note that you can only fetch a single sub-structure from a  `Document`  column per query. That is, you may not do the following:
```dart
// Invalid
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["type"], e.contents["user"]]);
```
For operations not supported by  `Query<T>`, you may use SQL directly:
final eventTagCounts = await context.persistentStore.execute("SELECT jsonb_array_length(contents->'tags') from _Event");













<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg0MzIwMTE1OSwtNjU4NzI4NzA0LDQ5MT
A3MDc4MV19
-->

## Multipart:
### Multipart header
#### [What should a Multipart HTTP request with multiple files look like? [duplicate]](https://stackoverflow.com/questions/913626/what-should-a-multipart-http-request-with-multiple-files-look-like)

#### There are  [three possibilities](https://www.w3.org/TR/html5/sec-forms.html#element-attrdef-form-enctype)  for  `enctype`:

-   [`application/x-www-form-urlencoded`](https://www.w3.org/TR/html5/sec-forms.html#urlencoded-form-data)
-   [`multipart/form-data`](https://www.w3.org/TR/html5/sec-forms.html#multipart-form-data)  (spec points to  [RFC7578](https://tools.ietf.org/html/rfc7578))
-   [`text/plain`](https://www.w3.org/TR/html5/sec-forms.html#plain-text-form-data). This is "not reliably interpretable by computer", so it should never be used in production, and we will not look further into it.

#### When you are writing client-side code:

-   use  **`multipart/form-data`**  when your form includes any  `<input type="file">`  elements
-   otherwise you can use  **`multipart/form-data`**  or  **`application/x-www-form-urlencoded`**  but  **`application/x-www-form-urlencoded`**  will be more efficient

	`enctype='multipart/form-data`  is an encoding type that allows files to be sent through a  _POST_. Quite simply, without this encoding the files cannot be sent through  _POST_. If you want to allow a user to upload a file via a form, you must use this  _enctype_.

_what a multipart header looks like_
```http
POST / HTTP/1.1
[[ Less interesting headers ... ]]
Content-Type: multipart/form-data; boundary=---------------------------735323031399963166993862150
Content-Length: 834

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="text1"

text default
-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="text2"

aωb
-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file1"; filename="a.txt"
Content-Type: text/plain

Content of a.txt.

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file2"; filename="a.html"
Content-Type: text/html

<!DOCTYPE html><title>Content of a.html.</title>

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file3"; filename="binary"
Content-Type: application/octet-stream

aωb
-----------------------------735323031399963166993862150--

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzkxMTIyOTQsMTY5MDg3OTM1MywtODc0OT
M4MjYzXX0=
-->

 ## Resource Controller:
## ResourceController

A  `ResourceController`  is a  [controller](https://aqueduct.io/docs/http/controller/)  that provide conveniences for **implementing endpoint controllers**. A  `ResourceController`  must be subclassed, and in that subclass, you write a method for each operation on that type of resource. For example, a  `UserController`  might handle the following operations:

-   creating a new user (`POST /users`)
-   getting all users (`GET /users`)
-   getting an individual user (`GET /users/:id`)
-   updating an individual user (`PUT /users/:id`)
-   deleting an individual user (`DELETE /users/:id`)

These methods that are invoked for an operation are called  _operation methods_.

### Operation Methods

> ⚡ An **operation method** is an instance method of a  **`ResourceController`**  subclass that has an  **`@Operation`**  annotation. It must return an instance of  **`Future<Response>`**. Here's an example:

```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities() async {
    return new Response.ok(["Atlanta", "Madison", "Mountain View"]);
  }
}
```

The above operation method will be invoked when  `CityController`  **handles  `GET`**  requests **without** path variables. To handle operation methods **with** path variables, the name of the path variable is added to the  `@Operation`  annotation:

```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities() async {
    return new Response.ok(["Atlanta", "Madison", "Mountain View"]);
  }
  @Operation.get('name')
  Future<Response> getCityByName() async {
    final id = request.path.variables['name'];
    return new Response.ok(fetchCityWithName(name));
  }
}
```

##### Path Variables

This controller would be linked to the route specification  `/cities/[:name]`, so that it can handle both of these operations. Read more about path variables in  [Routing](https://aqueduct.io/docs/http/routing/).

The named constructor of  `Operation`  tells us which HTTP method the operation method handles. The following named constructors exist:

-   `Operation.post()`
-   `Operation.get()`
-   `Operation.put()`
-   `Operation.delete()`

> ⚡ The canonical  `Operation()`  constructor takes the HTTP method as its first argument for **non-standard** operations, e.g.:
```dart
@Operation('PATCH', 'id')
Future<Response> patchObjectWithID() async => ...;
```
All  `Operation`  constructors take a variable **list** of **path variables**. There can be multiple path variables for an operation. An operation method will **only be invoked** if **all of its path variables** are present in the request path. There can be multiple operation methods for a given HTTP method, as long as each expects a different set of path variables.

**Here's an example of an operation that requires two path variables:**
```dart
@Operation.get('userID', 'itemID')
Future<Response> getUserItem() async {
  final userID = request.path.variables['userID'];
  final itemID = request.path.variables['itemID'];
  return new Response.ok(...);
}
```
If no operation method exists for a request, a **405** **Method Not Allowed response** is automatically sent and no operation method is invoked.

### Routing to a ResourceController

A  `ResourceController`  subclass must be preceded by a  `Router`  in the application channel. The  `Router`  will parse path variables so that the controller can use them to determine which operation method should be invoked. A typical route to a  `ResourceController`  contains an optional identifying path variable:
```dart
router
  .route("/cities/[:name]")
  .link(() => new CityController());
```
This route would allow  `CityController`  to implement operation methods for all HTTP methods with both no path variables and the 'name' path variable.

It is considered good practice to break sub-resources into their own controller. For example, the following is preferred:
```dart
router
  .route("/cities/[:name]")
  .link(() => new CityController());

router
  .route("/cities/:name/attractions/[:id]")
  .link(() => new CityAttractionController());
```
By contrast, the route  `/cities/[:name/[attractions/[:id]]]`, while valid, makes controller logic much more unwieldy.

### Request Bindings

Operation methods may  _bind_  properties of an HTTP request to its parameters. When the operation method is invoked, the **value of that property** is passed as an **argument to the operation method**. 

> ⚡ For example, the following binds the header named **'X-API-Key'** to the argument  **`apiKey`**:
```dart
@Operation.get('name')
Future<Response> getCityByName(@Bind.header('x-api-key') String apiKey) async {
  if (!isValid(apiKey)) {
    return new Response.unauthorized();
  }

  return new Response.ok(...);
}
```

> ⚡ The following table shows the possible types of bindings:
>  

| property            | binding                         |
|---------------------|---------------------------------|
| Path Variable       | @Bind.path(pathVariableName)    |
| URL Query Parameter | @Bind.query(queryParameterName) |
| Header              | @Bind.header(headerName)        |
| Request Body        | @Bind.body()                    |

 -----------------------------------------

You may bind any number of HTTP request properties to a single operation method.


#### Optional Bindings

> ⚡ Bindings can be made optional. If a binding is optional, the operation method will still be called even if the bound property isn't in a request. To make a binding optional, move it to the optional parameters of an operation method:
```dart
@Operation.get()
Future<Response> getAllCities({@Bind.header('x-api-key') String apiKey}) async {
  if (apiKey == null) {
    // No X-API-Key in request
    ...
  }
  ...
}
```
A bound parameter will be null if not in the request. Like any other Dart optional parameter, you can provide a default value:
```dart
@Operation.get()
Future<Response> getAllCities({@Bind.header("x-api-key") String apiKey: "public"}) async {
  ...
}
```
#### Automatically Parsing Bindings

Query, header and path bindings can automatically be parsed into other types, such as  `int`  or  `DateTime`. Simply declare the bound parameter's type to the desired type:
```dart
Future<Response> getCityByID(@Bind.query('id') int cityID)
```
The type of a bound parameter may be  `String`  or any type that implements  `parse`  (e.g.,  `int`,  `DateTime`). Query parameters may also be bound to  `bool`  parameters; a boolean query parameter will be true if the query parameter has no value (e.g.  `/path?boolean`).

If parsing fails for any reason, an error response is sent and the operation method is not called. For example, the above example binds  `int cityID`  - if the path variable 'id' can't be parsed into an  `int`, a 404 Not Found response is sent. If a query parameter or header value cannot be parsed, a 400 Bad Request response is sent.

You may also bind  `List<T>`  parameters to headers and query parameters, where  `T`  must meet the same criteria as above. Query parameters and headers may appear more than once in a request. For example, the value of  `ids`  is  `[1, 2]`  if the request URL ends with  `/path?id=1&id=2`  and the operation method looks like this:
```dart
Future<Response> getCitiesByIDs(@Bind.query('id') List<int> ids)
```
Note that if a parameter is  _not_  bound to a list and there are multiple occurrences of that property in the request, a 400 Bad Request response will be sent. If you want to allow multiple values, you must bind to a  `List<T>`.

#### Header Bindings

The following operation method binds the header named  `X-API-Key`  to the  `apiKey`  parameter:
```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities(@Bind.header('x-api-key') String apiKey) async {
    if (!isValid(apiKey)) {
      return new Response.unauthorized();
    }

    return new Response.ok(['Atlanta', 'Madison', 'Mountain View']);
  }
}
```
If an  `X-API-Key`  header is present in the request, its value will be available in  `apiKey`. If it is not,  `getAllCities(apiKey)`  would not be called and a 400 Bad Request response will be sent. If  `apiKey`  were optional, the method is called as normal and  `apiKey`  is null or a default value.

Header names are case-insensitive per the HTTP specification. Therefore, the header name may be 'X-API-KEY', 'X-Api-Key' 'x-api-key', etc. and  `apiKey`  will be bound in all cases.

#### Query Parameter Bindings

The following operation methods binds the query parameter named 'name' to the parameter  `cityName`:
```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities(@Bind.query('name') String cityName) async {
    return new Response.ok(cities.where((c) => c.name == cityName).toList());
  }
}
```
Query parameters can be required or optional. If required, a 400 Bad Request response is sent and no operation method is called if the query parameter is not present in the request URL. If optional, the bound variable is null or a default value.

Query parameters are case-sensitive; this binding will only match the query parameter 'name', but not 'Name' or 'NAME'.

Query parameters may also bound for query strings in the request body when the content-type is 'application/x-www-form-urlencoded'.

#### Path Variable Bindings

The following operation method binds the path variable 'id' to the parameter  `cityID`:
```dart
class CityController extends ResourceController {
  @Operation.get('id')
  Future<Response> getCityByID(@Bind.path('id') String cityID) async {
    return new Response.ok(cities.where((c) => c.id == cityID).toList());
  }
}
```
Path variables are made available when creating  [routes](https://aqueduct.io/docs/http/routing/). A  `Router`  must have a route that includes a path variable and that path variable must be listed in the  `Operation`  annotation. Path variables are case-sensitive and may not be optional.

If you attempt to bind a path variable that is not present in the  `Operation`, you will get a runtime exception at startup. You do not have to bind path variables for an operation method to be invoked.

#### HTTP Request Body Bindings

The body of an HTTP request can also be bound to a parameter:
```dart
class CityController extends ResourceController {
  CityController(this.context);

  final ManagedContext context;

  @Operation.post()
  Future<Response> addCity(@Bind.body() City city) async {
    final insertedCity = await context.insertObject(city);

    return new Response.ok(insertedCity);
  }
}
```
Since there is only one request body,  `Bind.body()`  doesn't take any identifying arguments.

> ⚡ The bound parameter type (**`City`**  in this example) must implement  **`Serializable`**. This interface requires two methods to be implemented: 
>  - **read data from a request body** 
>  - **write data to a response body**. 
>  Here is an example : 
```dart
class City extends Serializable {
  int id;
  String name;

  @override
  void readFromMap(Map<String, dynamic> map) {
    id = map['id'];
    name = map['name'];
  }

  @override
  Map<String, dynamic> asMap() {
    return {
      'id': id,
      'name': name
    }
  }
}
```


> 🔥 **ManagedObject and Serializable**
> `ManagedObject`s from Aqueduct's ORM implement  `Serializable`  without having to implement these two methods.

Aqueduct will automatically decode the request body from it's content-type, create a new instance of the bound parameter type, and invoke its  `readFromMap`  method. In the above example, a valid request body would be the following JSON:
```json
{
  "id": 1,
  "name": "Atlanta"
}
```
HTTP Body Decoding

Request bodies are decoded according to their content-type prior to being deserialized. For more information on request body decoding, including decoding content-types other than JSON, see  [this guide](https://aqueduct.io/docs/http/request_and_response/).

If parsing fails or  `readFromMap`  throws an exception, a 400 Bad Request response will be sent and the operation method won't be called.

You may also bind  `List<Serializable>`  parameters to the request body. Consider the following JSON that contains a list of cities:
```json
[
  {"id": 1, "name": "Atlanta"},
  {"id": 2, "name": "Madison"}
]
```
This body can be bound by declaring the bound parameter to be a  `List`  of the desired type:

Future<Response> addCity(@Bind.body() List<City> cities)

List vs Object

An endpoint should either take a single object or a list of objects, but not both. If the request body is a JSON list and the bound variable is not a list, a 400 Bad Request response will be sent (and vice versa). Declaring a body binding of the appropriate type validates the expected value and aids in automatically generating an OpenAPI specification for your application.

Note that if the request's  `Content-Type`  is 'x-www-form-urlencoded', its must be bound with  `Bind.query`  and not  `Bind.body`.

#### 🔥 Property Binding

The properties of an  `ResourceController`s may also have  `Bind.query`  and  `Bind.header`metadata. This binds values from the request to the  `ResourceController`  instance itself, making them accessible from  _all_  operation methods.
```dart
class CityController extends ResourceController {
  @requiredBinding
  @Bind.header("x-timestamp")
  DateTime timestamp;

  @Bind.query("limit")
  int limit;

  @Operation.get()
  Future<Response> getCities() async {
      // can use both limit and timestamp
  }
}
```
In the above, both  `timestamp`  and  `limit`  are bound prior to  `getCities`  being invoked. By default, a bound property is optional. Adding an  `requiredBinding`  annotation changes a property to required. If required, any request without the required property fails with a 400 Bad Request status code and none of the operation methods are invoked.


### Other ResourceController Behavior

Besides binding,  `ResourceController`s have some other behavior that is important to understand.

#### Request and Response Bodies

##### acceptedContentTypes

> ⚡ An  `ResourceController`  **can limit the content type of HTTP request bodies it accepts.** By default, an  `ResourceController`  will accept only  **`application/json`**  request bodies for its  **`POST`**  and  **`PUT`**  methods. This can be modified by setting the  **`acceptedContentTypes`**  property in the constructor.
```dart
class UserController extends ResourceController {
  UserController() {
    acceptedContentTypes = [ContentType.JSON, ContentType.XML];
  }
}
```
> ⚡ If a request is made with a content type other than the accepted content types, the controller automatically responds with a **415 Unsupported Media Type response.**

The body of an HTTP request is decoded if the content type is accepted and there exists a operation method to handle the request. The body is not decoded if there is not a matching operation method for the request. The body is decoded by  `ResourceController`  prior to your operation method being invoked. Therefore, 

> ⚡ you can always use the synchronous  **`RequestBody.as`**  method to access the body from within an operation method:
```dart
@Operation.post()
Future<Response> createThing() async {
  // do this:
  Map<String, dynamic> bodyMap = request.body.as();

  // no need to do this:
  Map<String, dynamic> bodyMap = await request.body.decode();

  return ...;
}
```
An  **`ResourceController`**  can also have a default content type for its responses. By default, this is  **`application/json`**. This default can be changed by changing  **`responseContentType`**  in the constructor:
```dart
class UserController extends ResourceController {
  UserController() {
    responseContentType = ContentType.XML;
  }
}
```
##### responseContentType

> ⚡ The  **`responseContentType`**  is the  _default_  response content type. An individual  `Response`  may set its own  `contentType`, which takes precedence over the  `responseContentType`. For example, the following controller returns **JSON** by default, but if the request specifically asks for XML, that's what it will return:
```dart
class UserController extends ResourceController {
  UserController() {
    responseContentType = ContentType.JSON;
  }

  @Operation.get('id')
  Future<Response> getUserByID(@Bind.path('id') int id) async {
    var response = new Response.ok(...);

    if (request.headers.value(Bind.headers.ACCEPT).startsWith("application/xml")) {
      response.contentType = ContentType.XML;
    }

    return response;
  }
}
```
#### More Specialized ResourceControllers

Many  `ResourceController`  subclasses will execute  [queries](https://aqueduct.io/docs/db/executing_queries/). There are helpful  `ResourceController`  subclasses for reducing boilerplate code.

##### QueryController<T>

> ⚡ A  `QueryController<T>`  builds a  **`Query<T>`**  based on the **incoming request.** 
> - If the request has a **body**, this  `Query<T>`'s  **`values`**  property is read from that body. 
>       --> where((u) => u.id).equalTo(id)
> - If the request has a **path variable**, the  `Query<T>`  assigns an expression to the **primary key value**. 
>       --> ..values = user;

> ⚡ ### highly recommended not to use queryController
>queryController 只能用在primaryKey與path一樣的條件下，不建議使用， 會有一些隱徵的問題出現，如path不是primary key 但該column有設indexed: true

For example, in a normal  `ResourceController`  that responds to a PUT request, you might write the following:
```dart
@Operation.put('id')
Future<Response> updateUser(@Bind.path('id') int id, @Bind.body() User user) async {
  var query = new Query<User>(context)
    ..where((u) => u.id).equalTo(id)
    ..values = user;

  return new Response.ok(await query.updateOne());
}
```
A  `QueryController<T>`  builds this query before a operation method is invoked, storing it in the inherited  `query`  property. A  `ManagedObject<T>`  subclass is the type argument to  `QueryController<T>`.
```dart
class UserController extends QueryController<User> {
  UserController(ManagedContext context) : super(context);

  @Operation.put('id')
  Future<Response> updateUser(@Bind.path('id') int id) async {
    // query already exists and is identical to the snippet above
    var result = await query.updateOne();
    return new Response.ok(result);
  }
}
```



A  `ManagedObjectController<T>`  can also be subclassed. 

> ⚡ A subclass allows for callbacks to be overridden to **adjust** the query **before execution**, or the results **before sending** the respond. 
> ⚡ Each operation - fetch, update, delete, etc. - has a pair of methods to do this. For example, the following subclass alters the query and results before any update via  `PUT`:

```dart
class UserController extends ManagedObjectController<User> {
  UserController(ManagedContext context) : super(context);

  Future<Query<User>> willUpdateObjectWithQuery(
      Query<User> query) async {
    query.values.lastUpdatedAt = new DateTime.now().toUtc();
    return query;
  }

  Future<Response> didUpdateObject(User object) async {
    object.removePropertyFromBackingMap("private");
    return new Response.ok(object);
  }
}
```
See the chapter on  [validations](https://aqueduct.io/docs/db/validations/), which are powerful when combined with  `ManagedObjectController<T>`.














<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNDE3Njg5MTUsMTkwMDIxMzQ4MCwxMj
A1OTQwMjI3LC01NDE5Njg3MDEsMTMzMzIzNTA1NiwtNjE0NDE5
NzE1LC0xMjU5NDAwMjc1LDIwMzM3MDM0OCw1MDc3Njg3NzAsLT
U2NzQzOTE3Niw0MTMwMzE0MTFdfQ==
-->

## Serving Files:


## Serving Files and Caching

Aqueduct can serve files by returning the contents of a file as an HTTP response body.

### FileController

> ✒️ Instances of  `FileController`  **serve a directory** from the filesystem through an HTTP interface. **Any route that channels requests to an  `FileController`**  _must_contain a  `*`  match-all token.

```dart
@override
Controller get entryPoint {
  final router = new Router();
  router.route("/files/*").link(() => new FileController("public/"));
  return router;
}
```
> ✒️ The argument to  `FileController`  is the **directory on the filesystem** in which request paths will be resolved against. In the above example, an HTTP request with the path  `/files/image.jpg`  would return the contents of the file  `public/image.jpg`.

> ⚡ Note that  **`public/`**  **does not** have a **leading slash** - therefore, the directory  `public`must be relative to the directory that the Aqueduct application was served from. 

In practice, this means you might have a directory structure like:
```
project/
  pubspec.yaml  
  lib/
    channel.dart
    ...
  test/
    ...
  public/
    image.jpg
```
Adding a **leading slash** to the directory served by  `FileController`  will resolve it relative to the **filesystem root**.

If the requested path was a directory, the filename  `index.html`  will be appended to the path when searching for a file to return.

If a file does not exist, an  `FileController`  returns a 404 Not Found response.

#### Content-Type of Files
##### setContentTypeForExtension - add content-types for extensions

An  `FileController`  will set the **content-type of the HTTP response** based on the served **files path** extension. By default, it recognizes many common extensions like  `.html`,  `.css`,  `.jpg`,  `.js`. You may add content-types for extensions to an instance:

```dart
var controller = new FileController("public/")
  ..setContentTypeForExtension("xml", new ContentType("application", "xml"));
```

> ✒️ If there is no entry for an extension of a file being served, the content-type defaults to  
`application/octet-stream`. An  `FileController`  will **never** invoke any **encoders** from  `CodecRegistry`, but it will **GZIP** data if the repository allows compression for the content-type of the file (see  `CodecRegistry.add`  and  `CodecRegistry.setAllowsCompression`).


### Caching
 
An  `FileController`  always sets the the Last-Modified header of the response to the last modified date according to the filesystem. If a request sends an If-Modified-Since header and the file has not been modified since that date, a 304 Not Modified response is sent with the appropriate headers.

You may provide Cache-Control headers depending on the path of the file being served. Here's an example that adds  `Cache-Control: public, max-age=31536000`

```dart
var policy = new CachePolicy(expirationFromNow: new Duration(days: 365));
var controller = new FileController("public/")
  ..addCachePolicy(policy, (path) => path.endsWith(".css"));
```

### File Serving and Caching Outside of FileController

A file can be served by any controller by setting the body object of a  `Response`  with its contents:
```dart
var file = new File("index.html");

// By loading contents into memory first...
var response = new Response.ok(file.readAsStringSync())
  ..contentType = new ContentType("application", "html");

// Or by streaming the contents from disk
var response = new Response.ok(file.openRead())
  ..encodeBody = false
  ..contentType = new ContentType("application", "html");
```

It is important to understand the how Aqueduct  [uses content-types to manipulate response bodies](https://aqueduct.io/docs/http/request_and_response/)  to serve file contents.

You may set the  `CachePolicy`  of any  `Response`. Note that  `CachePolicy`  only modifies the Cache-Control header of a response - headers like Last-Modified and ETag are not added.
```dart
var response = new Response.ok("contents")
  ..cachePolicy = new CachePolicy();
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NTkzNTUwNjYsMTI3NjAyMDUxNiwxNz
Q2NjAwNjYzLDE5NzkzMDU0MDZdfQ==
-->

## Validation:
 ## Validating Data

The preferred way of setting a validation is to add  `Validate`  metadata to properties of a table definition. Here's an example of a validation that ensures a tweet is less than 140 characters:
```dart
class Tweet extends ManagedObject<_Tweet> implements _Tweet {}
class _Tweet {
  @primaryKey
  int id;

  @Validate.length(lessThan: 140)
  String message;
}
```
#### Built-in Validators

There are a handful of built-in validations for common operations. For example, it is common to apply a regular expression to a value to ensure it formatted correctly or restrict the possible values to a list of available options. Common validators are available as named constructors of the  `Validate`  class. Here is an example:
```dart
class _Story {
  @primaryKey
  int id;

  @Validate.oneOf(const ["started", "accepted", "rejected", "delivered"])
  String state;
}
```
> ⚡ A built-in validator is useful because it automatically generates an error message that is returned in an HTTP response. For example, the previous code snippet indicates that the  `state`  property must be one of the four listed strings. If a value other than one of those four strings is used, the error message returned to the HTTP client would be:

**"The value `invalidValue` is not valid for `state`. Valid values are: 'started', 'accepted', 'rejected', 'delivered'.".**

See the API reference for  `Validate`  and its named constructors for possible options.
 `Validate`  annotations on transient properties have no effect. This annotation is only valid for properties declared in a table definition.


#### ⚡ Custom Validators

There will be times where the built-in validators are not sufficient for your application's use case. You may create subclasses of  `Validate`  to provide custom validation behavior. For example, if there were a  **`ValidatePhoneNumber`**  class:
```dart
class _Person {
  @primaryKey
  int id;

  @ValidatePhoneNumber()
  String phoneNumber;
}
```
A subclass of  `Validate`  must override  `Validate.validate()`  and call the superclass' primary constructor when instantiated. Here's an example of that phone number validator:
```dart
class ValidatePhoneNumber extends Validate {
  ValidatePhoneNumber({bool onUpdate: true, bool onInsert: true}) :
    super(onUpdate: onUpdate, onInsert: onInsert);

  @override
  void validate(ValidationContext context, dynamic value) {  
    if (value.length != 15) {
      context.addError("must be 15 digits");      
    }

    if (containsNonNumericValues(value)) {
      context.addError("must contain characters 0-9 only.");      
    }
  }
}
```

If  `value`  is doesn't meet the validation criteria, this method adds an error string to the  `ValidationContext`  it is passed. Error messages should be brief and indicate the successful criteria that failed. 

> ⚡ **Information about the property being validated will automatically** be added to the error message, so you **do not need** to include that information. If the context **has no errors** at the end of validation, the validation succeeds; otherwise, it fails.

A  `ValidationContext`  also has information about the property being validated, and whether the validation is running for an object being inserted or an object being updated.




#### Validation Behavior

A property may have more than one  `Validate`  metadata. In this case, all of the validations for a property must pass. The order in which multiple validations are performed is undefined and subject to change. Here's an example of validations that ensure a property's value is 10 characters long and only contains 10 alphabetic capital letters:
```dart
// property validator
@Validate.length(equalTo: 10)
@Validate.matches(r"$[A-Z]+^")
String tenCapitalLetters;
```

> ⚡ By default, validations are executed when a  `Query<T>`'s  **`insert`**  or  **`update`**  method is invoked. A validator can be restricted to only run on  **`insert`**  or  **`update`**  by passing values for its optional constructor arguments  **`onUpdate`**  and  **`onInsert`**:
```dart
// property validator
@Validate.matches(r"^[A-Z]+$", onInsert: true, onUpdate: false)
String validateOnInsertOnly;
```

It is important to understand how validations work when a value for a property is  **_not_**  specified in an insert or update query. For example, consider a  `Person`  with a  `name`  and  `email`  property and then inserted in a query where  `email`  hasn't been set:
```dart
var query = new Query<Person>(context)
  ..values.name = "Bob";

await query.insert();
```
Because  `email`  was not set on  `Query.values`, validations will not be run on that property.


**to require a property to be set: ** 
```dart
@Validate.present(onUpdate: false, onInsert: true)
String email;
```
> ⚡ The inverse of  `Validate.present()`  is  `Validate.absent()`. This validation **prevents a property from being set**. This is useful when a value should be included **during insertion**, but **can't be updated**. Here's an example:
```dart
@Validate.absent(onUpdate: true, onInsert: false)
String canOnlyBeSetOnce;
```

In the above declaration, the validator is only run on update operations and ensures that the property  `canOnlyBeSetOnce`  does not have a value. Because this validator is not run on insert operations, there is no restriction when the object is first inserted.

Validators are not run when a value is null. For example, the following insertion explicitly inserts  `null`  for the property  `email`:
```dart
var query = new Query<Person>(context)
  ..values.email = null
  ..values.name = "Bob";

await query.insert();
```
Nullability is enforced by  `Column.isNullable`  property. Consider the following declaration:
```dart
@Column(nullable: false)
@Validate.length(greaterThan: 10)
String name;
```


#### Other Validator Behavior

> ⚡ For validators that can't be built by subclassing  `Validate`, you may override  **`ManagedObject<T>.validate()`.** 
>  ⚡ This method is useful when a validation involves **more than one property**. Here's an example:
```dart
class Person extends ManagedObject<_Person> implements _Person {
  @override
  ValidationContext validate({Validating forEvent: Validating.insert}) {
   final ctx = super.validate(forEvent: forEvent);

    if (a + b > 10) {
      ctx.addError("a + b must be greater than 10");
    }

    return ctx;
  }
}
```
When overriding this method, the  `super`  implementation must be invoked to run validations managed by annotations. You must return the  `ValidationContext`  created by the superclass' implementation.

#### Skipping Validations

Validations are only run when values are set via  `Query<T>.values`. Values set via  `Query<T>.valueMap`  are not validated and is useful for inserting data without validation. Here's an example of skipping validation:
```dart
var query = new Query<Person>(context)
  ..valueMap = {
    "name" : "xyz",
    "email" : "whatever"
  };
```
#### Update and Insert Callbacks

> ⚡ `ManagedObject<T>`  subclasses may override  **`willUpdate`**  and  **`willInsert`**  to make changes prior to being updated or inserted. For example, a managed object may have updated and created dates that can be guaranteed to be set when inserted or updated:

```dart
class Person extends ManagedObject<_Person> implements _Person {
  @override
  void willUpdate() {
    updatedAt = new DateTime.now().toUtc();
  }

  @override
  void willInsert() {
    createdAt = new DateTime.now().toUtc();
  }
}
class _Person {
  @primaryKey
  int id;

  String name;
  DateTime createdAt;
  DateTime updatedAt;
}
```

Both  **`willUpdate`**  and  **`willInsert`**  are run **before any validation** occurs. Like validations,  `willUpdate`  and  `willInsert`  are skipped when using  `Query.valueMap`.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3MjU2NjE3NSwtMTEwNzU2NTQyNiw3OT
QyNzE4MjEsLTc4NzU4NDMzNCwxMDMxMzI2MjBdfQ==
-->
