




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

-------------------------------------------------------
### createApplicationRecord

added in  [API level 14](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
```java
public static createApplicationRecord (String packageName)
```
Create a new Android Application Record (AAR).

The Android Application Record (AAR) is a special type of [NDEF record](https://gototags.com/nfc/ndef/) that is used by Google’s Android operating system to signify to an NFC phone that an explicitly defined Android Application should be used to handle an NFC tag; this is called the [Tag Dispatch System](http://developer.android.com/guide/topics/connectivity/nfc/nfc.html). Often times an Android App record will be added as the **2nd NDEF record in the NDEF message**. This is useful to developers when they want to ensure their application is the application that will handle the NFC tag.

```kotlin
val sp_record = ...
val aap_record = NdefRecord.createApplicationRecord(packageName)  
val records = arrayOf(sp_record, aap_record)
```
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

----------------------------------
### Android Application Reccords (AAR)

added in  [API level 14](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
Android Application Record (AAR) provides a stronger certainty that your application is started when an `NFC tag is scanned`. An AAR has the package name of an application embedded inside an NDEF record. You can add an AAR to any NDEF record of your NDEF message, because Android searches the entire NDEF message for AARs. If it finds an AAR, it starts the application based on the package name inside the AAR. If the application is not present on the device, Google Play is launched to download the application.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzM4NDg5MTYzLDExMTU1ODcyMDQsMTIwMj
Q3ODUyOCwzNTA2NTE5ODddfQ==
-->