
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
```kotlin
   override fun onNewIntent(intent: Intent):Boolean {
      val nfc_intent = intent;
      super.onNewIntent(nfc_intent)
      // Set the pattern for vibration
      val pattern = longArrayOf(0, 100)

      // Set the initial auth parameters
      authStatus = AuthStatus.Disabled.value
      password = null

      // Vibrate on new Intent
      val vibrator = registrar.activity().getSystemService(Context.VIBRATOR_SERVICE) as Vibrator
      vibrator.vibrate(pattern, -1)
      doProcess(nfc_intent)
      return true;
   }
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
```kotlin
fun doProcess(nfc_intent: Intent) {
   nfcIntent = nfc_intent
   val tag = nfc_intent.getParcelableExtra<Tag>(NfcAdapter.EXTRA_TAG)
   demo = Ntag_I2C_Demo(tag, registrar.activity(), password, authStatus)
   if (demo!!.isReady) {
      // Retrieve Auth Status before doing any operation
      authStatus = obtainAuthStatus()
      val currTab = tabname
      launchDemo(currTab)
   }
}
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
```kotlin
if (currTab.data == "ntag_rf") {
   try {
      // SRAM Test
      if ((SpeedTestFragment.isSRamEnabled == true)) {
         // This demo is available even if the product is protected
         // as long as the SRAM is unprotected
         if ((authStatus == AuthStatus.Disabled.value
                         || authStatus == AuthStatus.Unprotected.value
                         || authStatus == AuthStatus.Authenticated.value
                         || authStatus == AuthStatus.Protected_W.value
                         || authStatus == AuthStatus.Protected_RW.value)) {
            demo!!.SRAMSpeedtest()
         } else {
            message?.onToastMakeText("NTAG I2C Plus memory is protected",Toast.LENGTH_LONG, this)
            showAuthDialog()
         }
      }
      // EEPROM Test
      if ((SpeedTestFragment.isSRamEnabled == false)) {
         // This demo is only available when the tag is not protected
         if ((authStatus == AuthStatus.Disabled.value
                         || authStatus == AuthStatus.Unprotected.value
                         || authStatus == AuthStatus.Authenticated.value)) {
            demo!!.EEPROMSpeedtest()
         } else {
            message?.onToastMakeText("NTAG I2C Plus memory is protected",Toast.LENGTH_LONG, this)
            showAuthDialog()
         }
      } // end if eeprom test
   } catch (e: Exception) {
      SpeedTestFragment.setAnswer(activity.getString(R.string.Tag_lost))
      e.printStackTrace()
   }
}
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
```kotlin
if (currTab.data == "leds") {
   //gordianknot....
   if (demo!!.isReady) {
      demo!!.finishAllTasks()
      if ( !demo!!.isConnected) {
         throw Exception("Demo should be connected")
      }
   }
   // This demo is available even if the product is protected
   // as long as the SRAM is unprotected
   if ((authStatus == AuthStatus.Disabled.value
                   || authStatus == AuthStatus.Unprotected.value
                   || authStatus == AuthStatus.Authenticated.value
                   || authStatus == AuthStatus.Protected_W.value
                   || authStatus == AuthStatus.Protected_RW.value)) {
      try {
         // if (LedFragment.getChosen()) {
         demo!!.LED()
      } catch (e: Exception) {
         e.printStackTrace()
         LedFragment.setAnswer(activity.getString(R.string.Tag_lost))
      }

   } else {
      message?.onToastMakeText("NTAG I2C Plus memory is protected", Toast.LENGTH_LONG, this)
      showAuthDialog()
   }
}
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
```kotlin
if (currTab.data == "ndef") {
    // This demo is only available when the tag is not protected
    if ((authStatus == AuthStatus.Disabled.value
                    || authStatus == AuthStatus.Unprotected.value
                    || authStatus == AuthStatus.Authenticated.value)) {
       NdefFragment.setAnswer("Tag detected")
       try {
          demo!!.NDEF()
       } catch (e: Exception) {
          // NdefFragment.setAnswer(getString(R.string.Tag_lost));
       }

    } else {
       message?.onToastMakeText("NTAG I2C Plus memory is protected",Toast.LENGTH_LONG, this)
       showAuthDialog()
    }
 }
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

```kotlin
fun launchNdefDemo(auth: Int, pwd: ByteArray) {
   if (demo!!.isReady) {
      if (demo!!.isConnected) {
         if (auth == AuthStatus.Authenticated.value) {
            demo!!.Auth(pwd, AuthStatus.Protected_RW.value)
         }
         NdefFragment.setAnswer("Tag detected")
         try {
            demo!!.NDEF()
         } catch (e: Exception) {
            NdefFragment.setAnswer("Error: Tag lost, try again")
            e.printStackTrace()
         }

      } else {
         if (NdefFragment.isWriteChosen) {
            NdefFragment.setAnswer("Tap tag to write NDEF content")
         } else {
            NdefFragment.setAnswer("Tap tag to read NDEF content")
         }
      }
   }
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMTE5MDUyOTAsLTIwNTA0NzM0NzgsLT
E5NzMyNjUyMzQsLTE1NjI3NzY3NjEsLTE0MTI5Mjk0MjcsLTk4
MzAzNTgzMSw2NzQ5NTkxNzQsMTM1Mzc2NjU0Myw0OTM4NDA4XX
0=
-->