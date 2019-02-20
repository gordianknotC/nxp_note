
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

### launchDemo
```mermaid
graph LR
authenticated --> demo.Auth
demo.Auth -.- $password
demo.Auth -.- ProtectedRW



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


LED -.- LedFragment
LedFragment --> Demo.isReady
Demo.isReady --> finishAllTask
finishAllTask --> assertConnected
LedFragment --> $unProte
LedFragment --> !unProte
$unProte --> demo.LED
demo.LED -.- err-taglost
!unProte --> Aler


NDEF -.- NdefFragment
NdefFragment --> $unProt
NdefFragment --> !unProt
$unProt--> msg.detected
$unProt--> demo.NDEF
!unProt --> Aler


CONFIG
```

### ShowXDialog
- FlashDialog
- AboutDialog
- HelpDialog
- DebugDialog
```mermaid
graph LR

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
eyJoaXN0b3J5IjpbMTk5NTg5NTgzLC0xNTYyNzc2NzYxLC0xND
EyOTI5NDI3LC05ODMwMzU4MzEsNjc0OTU5MTc0LDEzNTM3NjY1
NDMsNDkzODQwOF19
-->