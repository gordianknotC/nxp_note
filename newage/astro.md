
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
- param - **register** username
- param - **String** password
```mermaid
graph LR
subgraph feed request to auth/token and get response
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

response. -.-> !status200
!status200 -.-> error
response. -.-> $status200
$status200 -.-> response.body
response.body -.-> AuthorizationToken
```
### UserService.register
- param - **register** username
- param - **String** password
```mermaid
graph LR
subgraph feed request to register/ and get response
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

response. -.-> error
response. -.-> $status200
$status200 -.-> response.body
response.body -.-> AuthorizationToken
response. -.-> $status409
$status409 -.-> user_already_exists....
```


### UserService.getAuthenticatedUser
- param - **AuthorizationToken** token
```mermaid
graph LR
store -.-> executeUserRequest
executeUserRequest -.-> token
executeUserRequest -.-> request_url
token --> response
request_url --> response

response. -.-> $status200
response. -.-> error
$status200 -.-> response.body
$status200 -.-> token
token --> User
response.body --> User
```








### Store
```mermaid
graph TB

```












































<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NzIyMTQwMDgsMjc3NjgwNjAxLC0xOD
MyMzM1MzUwLC01MTA0NDc3MTgsNzEzNzYxMDE1LC0xOTk1NTQx
NTQ4LDExODA5Mjk5NTUsMTQ3Njc1NDk2MSwyMTQ3MjQyODgxLC
0xMTQwMzg1ODMzLDc3NjMyNzgwOCw3NTI5MzI0OCwyNDMxMDQ3
ODQsNjMyMDcwNjkzLDExMzU4MjExMzIsLTc0ODM1NDQxLC0xMT
kwMDIwMDY2LC0xMTQ4OTkwMjM3LC04NDkzMzE3NzgsMjA0MDI5
NzYyMl19
-->