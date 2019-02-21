



### NDEF()
read | write NDEF
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
$Write --> NDEFWrite
$Write --> ndefType
$Write --> $addAar

ndefType --> NdefText
ndefType --> NdefUri
ndefType --> NdefBT
ndefType --> NdefSp
NdefText --> createNdefTextMessage
NdefUri --> createNdefUriMessage
NdefBT --> createNdefBSSPMessage
NdefSp --> createNdefSpMessage


$addAar --> NdefRecord.createAar
NdefRecord.createAar -.- packageName
NdefRecord.createAar --> aarRecord

msg --> aarRecord
!Write --> NDEFReadTask




```
```kotlin

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc3NDk5MDY3NSwtODI4NTU2MzNdfQ==
-->