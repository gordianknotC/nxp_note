
--------------------------------------------

## Create common types of NDEF records
### createMime
Create a new NDEF Record containing MIME data.
```java
public static NdefRecord createMime (
	String mimeType, 
	byte[] mimeData
)
```
```kotlin
val mimeRecord = NdefRecord.createMime(
  "application/vnd.com.example.android.beam",
  "Beam me up, Android".toByteArray(Charset.forName("US-ASCII"))
)
```
```xml
<intent-filter>
    <action android:name="android.nfc.action.NDEF_DISCOVERED" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="application/vnd.com.example.android.beam"/>
</intent-filter>
```
.

----------------------------------
### createUri
```java
public static NdefRecord createUri (String uriString)
public static NdefRecord createUri (Uri uri)
```
```kotlin
val rtdUriRecord1  =  NdefRecord.createUri("http://example.com")
val rtdUriRecord2  =  Uri.parse("http://example.com").let { uri ->  		
	NdefRecord.createUri(uri)  
}
```
```xml
<intent-filter>
    <action android:name="android.nfc.action.NDEF_DISCOVERED" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="http"
        android:host="example.com"
        android:pathPrefix="" />
</intent-filter>
```
.

----------------------------------
### createExternal
Create a new NDEF Record containing external (application-specific) data

```java
public static NdefRecord createExternal (
	String domain, // domain-name of issuing organization
	String type,   // domain-specific type of data
	byte[] data	   // payload as bytes
)
```
> ⚡ NFC Forum requires that the domain and type used in an external record are treated as **`case insensitive`**, however Android intent filtering is always **`case sensitive`**. So this method will **`force the domain and type to` lower-case**  before creating the NDEF Record.
> 
```kotlin
var payload: ByteArray      //assign to your data
val domain = "com.example"  //usually your app's package name
val type = "externalType"   //will cast into lowercase (externaltype)
val extRecord = NdefRecord.createExternal(domain, type, payload)
```
```xml
<intent-filter>
    <action android:name="android.nfc.action.NDEF_DISCOVERED" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="vnd.android.nfc"
        android:host="ext"
        android:pathPrefix="/com.example:externaltype"/> 
</intent-filter>
```
> ⚡ pathprefix is in **lower-case**

.


-----------------------------------

### createTextRecord

added in  [API level 21](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
```java
public static NdefRecord createTextRecord (
	String languageCode, // Locale.TAIWAN.language.toString()
	String text
)
```
Create a new NDEF record containing UTF-8 text data.

The caller can either specify the language code for the provided text, or otherwise the language code corresponding to the current default locale will be used.  

> to specify languageCode
> ```kotlin
> Locale.TAIWAN.language.toString()
> ```

.

-------------------------------------------------------
### createApplicationRecord

added in  [API level 14](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
```java
public static createApplicationRecord (String packageName)
```
Create a new **Android Application Record** (AAR).

The Android Application Record (AAR) is a special type of [NDEF record](https://gototags.com/nfc/ndef/) that is used by Google’s Android operating system to signify to an NFC phone that an explicitly defined Android Application should be used to handle an NFC tag; this is called the [Tag Dispatch System](http://developer.android.com/guide/topics/connectivity/nfc/nfc.html). Often times an Android App record will be added as the **2nd NDEF record in the NDEF message**. This is useful to developers when they want to ensure their application is the application that will handle the NFC tag.

```kotlin
val sp_record = ...
val aap_record = NdefRecord.createApplicationRecord(packageName)  
val records = arrayOf(sp_record, aap_record)
```
.

----------------------------------
### Android Application Reccords (AAR)

Android Application Record (AAR) provides a stronger certainty that your application is started when an **`NFC tag is scanned`**. An AAR has the **`package name`** of an application **`embedded inside`** an NDEF record. You can add an **AAR** to **any** NDEF record of your NDEF message, because Android searches the **`entire NDEF message`** for **AARs**. If it finds an AAR, it starts the application based on the package name inside the AAR. If the application is not present on the device, Google Play is **`launched to download`** the application.

>AARs are useful if you want to **prevent other applications** from filtering for the same intent and potentially handling specific tags that you have deployed. AARs are _only supported at the **application** level_, because of the package name constraint, and not at the Activity level as with intent filtering. 

> 📘 **note** on _AAR only supported at application level_
> ```java
> public static createApplicationRecord (String packageName)
>```

If you want to handle an intent at the Activity level,  [use intent filters](https://developer.android.com/guide/topics/connectivity/nfc/nfc#filtering-intents).

- If a tag contains an AAR, the tag dispatch system dispatches in the following manner:

	1.  Try to start an Activity using an intent filter as normal. If the Activity that matches the intent also matches the AAR, start the Activity.
	2.  If the Activity that filters for the intent does not match the AAR, if multiple Activities can handle the intent, or if no Activity handles the intent, start the application specified by the AAR.
	3.  If no application can start with the AAR, go to Google Play to download the application based on the AAR.

>📘  **Note:** You can override AARs and the intent dispatch system with the [foreground dispatch system](https://developer.android.com/guide/topics/connectivity/nfc/advanced-nfc.html#foreground-dispatch), which allows a foreground activity to have priority when an NFC tag is discovered. With this method, the activity must be in the foreground to override AARs and the intent dispatch system.



.

--------------------------------------------


## Use the foreground dispatch system

The foreground dispatch system **`allows an activity to intercept an intent and claim priority`** over other activities that handle the same intent. Using this system involves constructing a few data structures for the Android system to be able to send the appropriate intents to your application. To enable the foreground dispatch system:

Add the following code in the `onCreate()` method of your activity:

1.  Create a  [PendingIntent](https://developer.android.com/reference/android/app/PendingIntent.html)` object so the Android system can populate it with the details of the tag when it is scanned.
	- Create a [PendingIntent](https://developer.android.com/reference/android/app/PendingIntent.html) object so the Android system can populate it with the details of the tag when it is scanned.
	```kotlin
		val intent = Intent(this, javaClass).apply {
		    addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP)
		}
		var pendingIntent: PendingIntent = PendingIntent.getActivity(this, 0, intent, 0)
	```

	- Declare intent filters to handle the intents that you want to intercept. The foreground dispatch system checks the specified intent filters with the intent that is received when the device scans a tag. If it matches, then your application handles the intent. If it does not match, the foreground dispatch system falls back to the intent dispatch system. Specifying a `null` array of intent filters and technology filters, specifies that you want to filter for all tags that fallback to the `TAG_DISCOVERED` intent. The code snippet below handles all MIME types for `NDEF_DISCOVERED`. You should only handle the ones that you need.
	```kotlin
		val ndef = IntentFilter(NfcAdapter.ACTION_NDEF_DISCOVERED).apply {
		    try {
		        addDataType("*/*")    /* Handles all MIME based dispatches.
		                                 You should specify only the ones that you need. */
		    } catch (e: IntentFilter.MalformedMimeTypeException) {
		        throw RuntimeException("fail", e)
		    }
		}
		intentFiltersArray = arrayOf(ndef)	
	```
	- Set up an array of tag technologies that your application wants to handle. Call the `Object.class.getName()` method to obtain the class of the technology that you want to support.
	```kotlin
		techListsArray = arrayOf(arrayOf<String>(NfcF::class.java.name))
	```
	
2.  Override the following activity lifecycle callbacks and add logic to enable and disable the foreground dispatch when the activity loses ([onPause()](https://developer.android.com/reference/android/app/Activity.html#onPause())) and regains ([onResume()](https://developer.android.com/reference/android/app/Activity.html#onResume())) focus.  [enableForegroundDispatch()](https://developer.android.com/reference/android/nfc/NfcAdapter.html#enableForegroundDispatch(android.app.Activity,%20android.app.PendingIntent,%20android.content.IntentFilter[],%20java.lang.String[][]))  must be called from the main thread and only when the activity is in the foreground (calling in  [onResume()](https://developer.android.com/reference/android/app/Activity.html#onResume()) guarantees this). You also need to implement the  [onNewIntent](https://developer.android.com/reference/android/app/Activity.html#onNewIntent(android.content.Intent))  callback to process the data from the scanned NFC tag.

	```kotlin
	public override fun onPause() {
	    super.onPause()
	    adapter.disableForegroundDispatch(this)
	}

	public override fun onResume() {
	    super.onResume()
	    adapter.enableForegroundDispatch(this, pendingIntent, intentFiltersArray, techListsArray)
	}

	public override fun onNewIntent(intent: Intent) {
	    val tagFromIntent: Tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG)
	    //do something with tagFromIntent
	}
	```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2MTIwNTk3MSw2NDE4Mzg5NywxMTE1NT
g3MjA0LDEyMDI0Nzg1MjgsMzUwNjUxOTg3XX0=
-->