
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
## noteService
### noteService.SetAuthenticateUser
param - **User** user
```mermaid
graph LR
_storedUserKey-.- user.json....
SetauthenticatedUser --> $User...
SetauthenticatedUser --> !User...
$User... --> storageProvider
storageProvider --> store
store --> _storedUserKey
store --> User...

!User... --> storageProvider.
storageProvider. --> delete
delete --> storageKey...
!User... --> noteController
noteController --> clearCache...
```

### noteContoller.executeClientRequest
param - **Request** request
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

### noteContoller.executeRequest
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

### noteContoller.executeUserRequest
param - Request request
param - AuthorizationToken token
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

### noteContoller._loadPersistentUser
```mermaid
graph LR
storageProvider -.- load
load --> |_storedUserKey| contents
contents --> authenticatedUser
UserService--> add
authenticatedUser --> add
```
## ServiceController
```mermaid
graph LR
	add --> eventName
```

## UserService
### UserService.login
```mermaid
graph LR
Store
POST
POST -.-> username
POST -.-> password
POST -.-> grand_type

```



### Store
```mermaid
graph TB

```












































<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIzNzkyMjMzMSwxMTgwOTI5OTU1LDE0Nz
Y3NTQ5NjEsMjE0NzI0Mjg4MSwtMTE0MDM4NTgzMyw3NzYzMjc4
MDgsNzUyOTMyNDgsMjQzMTA0Nzg0LDYzMjA3MDY5MywxMTM1OD
IxMTMyLC03NDgzNTQ0MSwtMTE5MDAyMDA2NiwtMTE0ODk5MDIz
NywtODQ5MzMxNzc4LDIwNDAyOTc2MjJdfQ==
-->