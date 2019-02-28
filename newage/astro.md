
### AuthorizationToken
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

### StorageProvider
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

### User
```dart
int id;  
String email;  
List<Note> notes = [];  
AuthorizationToken token;
bool get isAuthenticated => 
	token != null && !token.isExpired;
```

### noteService.SetAuthenticateUser
param - **User** user
```mermaid
graph LR
storeUserKey -.- user.json....
SetauthenticatedUser --> $User...
SetauthenticatedUser --> !User...
$User... --> storageProvider
storageProvider --> store
store --> storeUserKey
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
	!isExpired -->
```

### Store
```mermaid
graph TB

```

### UserService
```mermaid
graph TB
Store


```








































<!--stackedit_data:
eyJoaXN0b3J5IjpbOTY3MjA2OTc0LC0xMTQwMzg1ODMzLDc3Nj
MyNzgwOCw3NTI5MzI0OCwyNDMxMDQ3ODQsNjMyMDcwNjkzLDEx
MzU4MjExMzIsLTc0ODM1NDQxLC0xMTkwMDIwMDY2LC0xMTQ4OT
kwMjM3LC04NDkzMzE3NzgsMjA0MDI5NzYyMl19
-->