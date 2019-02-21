




### NDEF()
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

nde

```
```kotlin

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzNTAwMDc2NywtODI4NTU2MzNdfQ==
-->