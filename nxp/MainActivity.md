
### onCreate
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

### onPause
```mermaid
graph LR
	onPause --> $Adapter
	onPause --> !Adapter
	$Adapter --> disableForeGround
	$Adapter --> Demo.finisTask
```
### onActivityResult
```mermaid
graph LR
  demo.Ready --> launchDemo
  AuthRequest --> launchDemo
  
```

### onNewIntent
```mermaid
graph LR
	set --> $authStatus
	set --> $password
	set --> vibrator
	$nfcIntent --> doProcess
	doProcess --> $tag
	doProcess --> demo
	demo -.- $passwor
	demo -.- $authStatu
	demo -.- $tag
	demo -.- activity
	demo --> isReady
	isReady --> obtainAuth
	obtainAuth --> $authStatu
	isReady --> lauchDemo
```

#### doProcess
```mermaid
graph LR
nfcIntent --> tag
demo -.- tag
demo -.- password
demo -.- authStatus
demo --> Ready
Ready --> obtainAuthStatus
Ready --> launchDemo
obtainAuthStatus -.- authStatus
```

### launchDemo
```mermaid
graph LR
authenticated --> demo.Auth
demo.Auth -.- $password
demo.Auth -.- ProtectedRW
```

> #### launchDemo - SpeedTest
``` mermaid
graph LR
NTAGRF -.- SpeedTestFragment
SpeedTestFragment--> $SRAM
SpeedTestFragment--> !SRAM
$SRAM --> $unProtect
$SRAM --> !unProtect
$unProtect--> demo.SRAMSpeedtest
!unProtect--> Alert
!SRAM --> $unProtec
!SRAM --> !unProtec
$unProtec --> demo.EEPROMSpeedTest
!unProtec --> Alert
```

> #### launchDemo - LED test
``` mermaid
graph LR
LED -.- LedFragment
LedFragment --> Demo.isReady
Demo.isReady --> finishAllTask
finishAllTask --> assertConnected
LedFragment --> $unProte
LedFragment --> !unProte
$unProte --> demo.LED
demo.LED -.- err-taglost
!unProte --> Aler
```
> #### launchDemo - NDEF Test
``` mermaid
graph LR
NDEF -.- NdefFragment
NdefFragment --> $unProt
NdefFragment --> !unProt
$unProt--> msg.detected
$unProt--> demo.NDEF
!unProt --> Aler


CONFIG
``` 

### launchNdefDemo
```mermaid
graph LR
NdefFragment --> launchNdefDemo 
launchNdefDemo --> demo.connected
demo.connected --> authenticated
authenticated --> demo.Auth
authenticated --> tag_detected
authenticated --> demo.NDEF
launchNdefDemo --> !demo.connected
!demo.connected --> msg.tap_tag_to_read

```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NzMyNjUyMzQsLTE1NjI3NzY3NjEsLT
E0MTI5Mjk0MjcsLTk4MzAzNTgzMSw2NzQ5NTkxNzQsMTM1Mzc2
NjU0Myw0OTM4NDA4XX0=
-->