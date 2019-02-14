




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

>AARs are useful if you want to **prevent other applications** from filtering for the same intent and potentially handling specific tags that you have deployed. AARs are only supported at the **application** level, because of the package name constraint, and not at the Activity level as with intent filtering. 

> note:

If you want to handle an intent at the Activity level,  [use intent filters](https://developer.android.com/guide/topics/connectivity/nfc/nfc#filtering-intents).

If a tag contains an AAR, the tag dispatch system dispatches in the following manner:

1.  Try to start an Activity using an intent filter as normal. If the Activity that matches the intent also matches the AAR, start the Activity.
2.  If the Activity that filters for the intent does not match the AAR, if multiple Activities can handle the intent, or if no Activity handles the intent, start the application specified by the AAR.
3.  If no application can start with the AAR, go to Google Play to download the application based on the AAR.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU1NTg0MzEwLDY0MTgzODk3LDExMTU1OD
cyMDQsMTIwMjQ3ODUyOCwzNTA2NTE5ODddfQ==
-->