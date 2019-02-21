




### NDEF()
``` mermaid
graph LR
$Write
!Write

NdefFragment
NdefText
NdefUri
NdefBT
NdefSp
ndefType

$Write --> ndefType
ndefType --> NdefText
ndefType --> NdefUri
ndefType --> NdefBT
ndefType --> NdefSp
NdefText --> createNdefTextMessage
NdefUri --> createNdefUriMessage
NdefBT --> createNdefBSSPMessage
NdefSp --> createNdefSpMessage

```
```kotlin

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0ODU0ODA1NjgsLTgyODU1NjMzXX0=
-->