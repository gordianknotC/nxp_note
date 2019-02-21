



### NDEF()
read | write NDEF
``` mermaid
graph LR
 
$Write
!Write
$addAar

subgraph NdefFragment
	NdefText
	NdefUri
	NdefBT
	NdefSp
	ndefType
end

msg --> ndefType
$Write --> NDEFWrite
$Write --> ndefType
$Write --> $addAar
!Write --> NDEFReadTask

ndefType --> NdefText
ndefType --> NdefUri
ndefType --> NdefBT
ndefType --> NdefSp

NdefText --> createNdefTextMessage
NdefUri --> createNdefUriMessage
NdefBT --> createNdefBSSPMessage
NdefSp --> createNdefSpMessage
createNdefTextMessage --> msg0
createNdefUriMessage --> msg0
createNdefBSSPMessage --> msg0
createNdefSpMessage --> msg0

$addAar --> NdefRecord.createAar
NdefRecord.createAar -.- packageName
NdefRecord.createAar --> aarRecord

msg --> aarRecord
msg -.- NDEFWrite




```
msg: **NDEFMessage** | write: **NdefFragment.Boolean**

```kotlin

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMTgwMjI3NDgsLTgyODU1NjMzXX0=
-->