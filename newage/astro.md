
### AuthToken
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

### noteService.authenticateUser
param - **User** user
```mermaid
graph LR
storeUserKey -.- user.json....
SetauthenticatedUser --> $User...
SetauthenticatedUser --> !User...
!User... --> storageProvider
storageProvider --> store
store --> storeUserKey
store --> User...

$User... --> storageProvider.
storageProvider. --> delete
delete --> storageKey...
$User... --> noteController
noteController --> clearCache...
```

### noteContoller.executeClientRequest
param - **Request** request
```mermaid
graph LR
	subgraph RequestHeader
	request -.-|autherizationHeader| headers
	authString -.- headers
	requestHeader --> request
end
RequestHeader --> executeRequest
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
eyJoaXN0b3J5IjpbLTQ5NzE4NTQwNSw2MzIwNzA2OTMsMTEzNT
gyMTEzMiwtNzQ4MzU0NDEsLTExOTAwMjAwNjYsLTExNDg5OTAy
MzcsLTg0OTMzMTc3OCwyMDQwMjk3NjIyXX0=
-->