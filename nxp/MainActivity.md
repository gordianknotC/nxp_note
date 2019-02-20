




```mermaid
graph LR
	onCreate
	setNfcForeGround
	checkNFC


	onCreate --> setNfcForeGround
	setNfcForeGround --> checkNFC
	setNfcForeGround --> $pendingIntent
	checkNFC --> $Adapter
	$Adapter --> $enable
	$Adapter --> $disable
	$disable --> NoNfcAlert
	$Adapter --> $null
	$nu

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyNjExNzI4MCw0OTM4NDA4XX0=
-->