
### AuthToken
```mermaid
graph LR
access_token -.
refresh_token
expires_in
expiresAt

AuthToken --> access_token
AuthToken --> refresh_token
AuthToken -->|indicates duration| expires_in
AuthToken -->|expiration date| expiresAt

```
```mermaid
graph TB


```








































<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjA4MTA2NTQsLTExOTAwMjAwNjYsLT
ExNDg5OTAyMzcsLTg0OTMzMTc3OCwyMDQwMjk3NjIyXX0=
-->