
### AuthToken
```mermaid
graph LR
access_token -.- String
refresh_token -.- String
expires_in

AuthToken --- access_token
AuthToken --- refresh_token
AuthToken ---|indicates duration| expires_in
expires_in -.- Now
Now -.-|DateTime.toIso8601String| expiaresAt___
```
```mermaid
graph LR
AuthToken -.- isExpired
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
```mermaid
graph TB


```








































<!--stackedit_data:
eyJoaXN0b3J5IjpbMjE2MzE1MjM4LC03NDgzNTQ0MSwtMTE5MD
AyMDA2NiwtMTE0ODk5MDIzNywtODQ5MzMxNzc4LDIwNDAyOTc2
MjJdfQ==
-->