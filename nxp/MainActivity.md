
onCreate
```mermaid
graph LR
	onCreate
	setNfcForeGround
	checkNFC

	onCreate --> setNfcForeGround
	onCreate --> checkNFC
	setNfcForeGround --> $pendingIntent
	checkNFC --> $Adapter
	$Adapter --> $enable
	$Adapter --> $disable
	$disable --> NoNfcAlert
	$Adapter --> !Adapter
	!Adapter --> closeNoNfc
```


```mermaid
graph LR
	onPause --> $Adapter
	onPause --> !Adapter
	$Adap

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA3ODIxMTQxNCw0OTM4NDA4XX0=
-->