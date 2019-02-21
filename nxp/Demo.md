



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
eyJoaXN0b3J5IjpbLTMzMDU1MDgwMCwtODI4NTU2MzNdfQ==
-->