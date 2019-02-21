




### NDEF()
read NDEF / write
``` mermaid
graph LR
$Write
!Write
$addAar
!addAar

NdefFragment
NdefText
NdefUri
NdefBT
NdefSp
ndefType
msg --> ndefType

$Write --> ndefType
ndefType --> NdefText
ndefType --> NdefUri
ndefType --> NdefBT
ndefType --> NdefSp
NdefText --> createNdefTextMessage
NdefUri --> createNdefUriMessage
NdefBT --> createNdefBSSPMessage
NdefSp --> createNdefSpMessage

$Write --> $addAar
$addAar --> NdefRecord.createAar
NdefRecord.createAar -.- packageName
NdefRecord.createAar --> aarRecord

msg --> aarRecord
!Write --> NDEFReadTask


```
```kotlin

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNDU2MjIzNTUsLTgyODU1NjMzXX0=
-->