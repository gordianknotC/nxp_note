
## AuthorizationToken
```mermaid
graph LR
access_token -.- String
refresh_token -.- String
expires_in

AuthToken --- access_token
AuthToken --- refresh_token
AuthToken --- |indicates duration| expires_in
expires_in -.- Now
Now -.- |DateTime.toIso8601String| expiaresAt___
```
```mermaid
graph LR
AuthToken --- isExpired
```
```dart
fromMap() =>
{
	"access_token"..., "refresh_token"..., "expires_in": 3000
}
Map<String, dynamic> asMap() =>  
{  
	"access_token": accessToken,  
	"refresh_token": refreshToken,  
	"expiresAt": expiresAt.toIso8601String()  
};
```

## StorageProvider
 ```mermaid
graph LR
StorageProvider
load -.- path
store -.- path
delete -.- path
StorageProvider -.- load
StorageProvider -.- store
StorageProvider -.- delete

```

## User
```dart
int id;  
String email;  
List<Note> notes = [];  
AuthorizationToken token;
bool get isAuthenticated => 
	token != null && !token.isExpired;
```

## ServiceController
```mermaid
graph LR
	add --> eventName
```


## UserService: ServiceController
### UserService.login
- param - **register** username
- param - **String** password
- return - **Future(User)**
- vars - **Store** store
```mermaid
graph LR

subgraph 2-return User by getAuthenticatedUser
	response. -.-> !status200
	!status200 -.-> error
	response. -.-> $status200
	$status200 --> getAuthenticatedUser
	getAuthenticatedUser -.-> response.body
end
subgraph 1-feed request to auth/token and get response
	POST
	POST -.-> username
	POST -.-> password
	POST -.-> grand_type
	password --> request
	grand_type --> request
	username --> request

	Store
	Store -.-> executeClientRequest
	request --> executeClientRequest
	executeClientRequest -.-> response
end

```
```dart
Future<User> login(String username, String password) async {  
  var req = new Request.post("/auth/token", {  
    "username": username,  
  "password": password,  
  "grant_type": "password"  
  }, contentType: new ContentType("application", "x-www-form-urlencoded"));  
  var response = await store.executeClientRequest(req);  
  if (response.error != null) {  
    addError(response.error);  
  return null;  
  }  
  switch (response.statusCode) {  
    case 200: return getAuthenticatedUser(token: new AuthorizationToken.fromMap(response.body));  
  default: addError(new APIError(response.body["error"]));  
  }  
  return null;  
}
```


### UserService.register
- param - **register** username
- param - **String** password
- return - **Future\<User>**
```mermaid
graph LR

subgraph 1 - feed request to register/ and get response
	POST
	POST -.-> username
	POST -.-> password
	password --> request
	username --> request

	Store
	Store -.-> executeClientRequest
	request --> executeClientRequest
	executeClientRequest -.-> response
end

subgraph 2 - return User by getAuthenticatedUser
	response. -.-> error
	response. -.-> $status200
	$status200 -.-> response.body
	response.body -.-> AuthorizationToken
	response. -.-> $status409
	$status409 -.-> user_already_exists....
end
```


### UserService.getAuthenticatedUser
- param - **AuthorizationToken** token
```mermaid
graph LR
subgraph process response
	response. -.-> $status200
	response. -.-> error
	$status200 -.-> response.body
	$status200 -.-> token
	token --> User
	response.body --> User
end

subgraph get response with authToken
	store -.-> executeUserRequest
	executeUserRequest -.-> token
	executeUserRequest -.-> request_url
	token --> response
	request_url --> response
end
```
```mermaid
graph LR
subgraph broadcast user to stream
	User.... --> broadcast_to_Stream 
end
```

## Store
- vars - NoteService noteController
- vars - UserService userController



### Store.authenticatedUser
- param - **User** user
```mermaid
graph LR
	storageProvider -.-> store
	store -.-> User.asMap
	store -.-> _storedUserKey
	$User --> storageProvider
	!User --> noteController.clearCache
	!User --> storageProvider.delete
	storageProvider.delete -.- _storedUserKey.
```
### Store.executeClientRequest
- param - **Request** request
```mermaid
graph LR
	subgraph RequestHeader
	request -.-|autherizationHeader| authString 
end
RequestHeader --> executeRequest
```
```dart
Future<Response> executeClientRequest(Request request) async {  
  request.headers[HttpHeaders.authorizationHeader] = clientAuthorization;  
  return executeRequest(request);  
}
```

### Store.executeRequest
param - **Request** request
```mermaid
graph LR
subgraph GET
	GET. -.- RequestPath
	GET. -.- Header
end

subgraph POST
	POST. --> JSON
	JSON -.- body.
	body. -.- json.encodeBody...
	POST. --> AuthCodeLogin...
	AuthCodeLogin... -.- body
	body -.- Key&Value_String
end

RequestHeader --> executeRequest  
executeRequest --> GET
executeRequest --> POST
```

### Store.executeUserRequest
param - **Request** request
param - **AuthorizationToken** token
```mermaid
graph LR
	executeUserRequest --> request
	executeUserRequest --> token
	token --> $isExpired
	$isExpired --> Error
	token --> !isExpired
	!isExpired -->|set auth into request header| setHeader
	!isExpired --> executeRequest
```
```dart
Future<Response> executeUserRequest(Request request, {AuthorizationToken token}) async {  
  AuthorizationToken t = token ?? authenticatedUser?.token;  
  if (t?.isExpired ?? true) {  
    throw new UnauthenticatedException();  
  }  
  request.headers[HttpHeaders.authorizationHeader] = t.authorizationHeaderValue;  
  var response = await executeRequest(request);  
  if (response.statusCode == 401) {  
    // Refresh the token, try again  
  }  
  return response;  
}
```

### Store._loadPersistentUser
```mermaid
graph LR
storageProvider -.- load
load --> |_storedUserKey| contents
contents --> authenticatedUser
UserService--> add
authenticatedUser --> add










































<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzOTkzMTk3NDAsLTI3MTIzNzE3NSwtMj
U2NjIwMzc5LC0xMDY5MTI0NTEyLC0yODEyMTE5MDAsMTY1NDIx
NjI3LDk3MzAyMTQ2NCwtMTY5NTY2OTQwOCwtNjI0MTk2NjA0LD
I3NzY4MDYwMSwtMTgzMjMzNTM1MCwtNTEwNDQ3NzE4LDcxMzc2
MTAxNSwtMTk5NTU0MTU0OCwxMTgwOTI5OTU1LDE0NzY3NTQ5Nj
EsMjE0NzI0Mjg4MSwtMTE0MDM4NTgzMyw3NzYzMjc4MDgsNzUy
OTMyNDhdfQ==
-->