<!--#-->
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
