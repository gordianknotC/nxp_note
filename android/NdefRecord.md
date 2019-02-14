


### createApplicationRecord

added in  [API level 14](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
```java
public static createApplicationRecord (String packageName)
```
Create a new Android Application Record (AAR).

This record indicates to other Android devices the package that should be used to handle the entire NDEF message. You can embed this record anywhere into your message to ensure that the intended package receives the message.

When an Android device dispatches an  [NdefMessage](https://developer.android.com/reference/android/nfc/NdefMessage.html)  containing one or more Android application records, the applications contained in those records will be the preferred target for the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intent, in the order in which they appear in the message. This dispatch behavior was first added to Android in Ice Cream Sandwich.

If none of the applications have a are installed on the device, a Market link will be opened to the first application.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzUwNjUxOTg3XX0=
-->