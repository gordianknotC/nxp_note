
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

###
 
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
eyJoaXN0b3J5IjpbNjc4MjA5NTIwLDExMzU4MjExMzIsLTc0OD
M1NDQxLC0xMTkwMDIwMDY2LC0xMTQ4OTkwMjM3LC04NDkzMzE3
NzgsMjA0MDI5NzYyMl19
-->