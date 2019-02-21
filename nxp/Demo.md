




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

```
```kotlin

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY3NjUyNzQ4NSwtODI4NTU2MzNdfQ==
-->