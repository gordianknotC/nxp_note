
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
StorageProvider-.- load
StorageProvider -.- store
StorageProvider -.- 
```

### noteService
```mermaid
graph LR


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
eyJoaXN0b3J5IjpbLTE3NDQzNzg1NywxMTM1ODIxMTMyLC03ND
gzNTQ0MSwtMTE5MDAyMDA2NiwtMTE0ODk5MDIzNywtODQ5MzMx
Nzc4LDIwNDAyOTc2MjJdfQ==
-->