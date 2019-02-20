
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

```kotlin
private fun checkNFC() {  
   if (mAdapter != null) {  
      if (!mAdapter!!.isEnabled) {  
         Dart.showSettingNoNfcAlert(this)  
      }  
   } else {  
      Dart.closeAppNoNfcAlert(this)  
   }  
}
fun setNfcForeground() {
  mPendingIntent = PendingIntent.getActivity(
  registrar.activity(), 0,
  Intent(registrar.context(), javaClass)
       .addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP), 0)
   }
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
> call from NdefFragment
```mermaid
graph LR
launchNdefDemo 
launchNdefDemo --> demo.connected
demo.connected --> authenticated
authenticated --> demo.Auth
authenticated --> tag_detected
authenticated --> demo.NDEF
launchNdefDemo --> !demo.connected
!demo.connected --> msg.tap_tag_to_read

```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY5MjQxNjc4MiwtMjA1MDQ3MzQ3OCwtMT
k3MzI2NTIzNCwtMTU2Mjc3Njc2MSwtMTQxMjkyOTQyNywtOTgz
MDM1ODMxLDY3NDk1OTE3NCwxMzUzNzY2NTQzLDQ5Mzg0MDhdfQ
==
-->