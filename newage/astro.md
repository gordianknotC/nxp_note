
### AuthToken
```mermaid
graph LR
access_token -.- String
refresh_token -.- String
expires_in


AuthToken --> access_token
AuthToken --> refresh_token
AuthToken -->|indicates duration| expires_in
expires_in -.- Now
Now -.-

```
```mermaid
graph TB


```








































<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NDUwNzk3NTMsLTc0ODM1NDQxLC0xMT
kwMDIwMDY2LC0xMTQ4OTkwMjM3LC04NDkzMzE3NzgsMjA0MDI5
NzYyMl19
-->