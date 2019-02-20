




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
	$disable --> DartAlert


```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzkzNjEwMzk4LDQ5Mzg0MDhdfQ==
-->