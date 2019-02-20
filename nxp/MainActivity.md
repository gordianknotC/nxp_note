




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
	$Adapter --> !Adapter
	!Adapter --> 

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNzQxMTA1MTQsNDkzODQwOF19
-->