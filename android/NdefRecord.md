


### createApplicationRecord

added in  [API level 14](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
```java
public static createApplicationRecord (String packageName)
```
Create a new Android Application Record (AAR).

The Android Application Record (AAR) is a special type of [NDEF record](https://gototags.com/nfc/ndef/) that is used by Google’s Android operating system to signify to an NFC phone that an explicitly defined Android Application should be used to handle an NFC tag; this is called the [Tag Dispatch System](http://developer.android.com/guide/topics/connectivity/nfc/nfc.html). Often times an Android App record will be added as the 2nd NDEF record in the NDEF message. This is useful to developers when they want to ensure their application is the application that will handle the NFC tag.

```kotlin
val sp_record = NdefRecord(  
        NdefRecord.TNF_WELL_KNOWN,  
  NdefRecord.RTD_SMART_POSTER, ByteArray(0),  
  spMessage.toByteArray())  
val aap_record = NdefRecord.createApplicationRecord(packageName)  
val records = arrayOf(sp_record, aap_record)
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NjQ2OTU0LDM1MDY1MTk4N119
-->