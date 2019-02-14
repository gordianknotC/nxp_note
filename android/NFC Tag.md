
 # Tag

public final class Tag  
extends  [Object](https://developer.android.com/reference/java/lang/Object.html?hl=CA) implements  [Parcelable](https://developer.android.com/reference/android/os/Parcelable.html?hl=CA)

[java.lang.Object](https://developer.android.com/reference/java/lang/Object.html?hl=CA)

↳

android.nfc.Tag

  

----------

Represents an NFC tag that has been discovered.

`[Tag](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA)`  is an immutable object that represents the state of a NFC tag at the time of discovery. It can be used as a handle to  `[TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)`  classes to perform advanced operations, or directly queried for its ID via  `[getId()](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA#getId())`  and the set of technologies it contains via  `[getTechList()](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA#getTechList())`. Arrays passed to and returned by this class are  _not_  cloned, so be careful not to modify them.

A new tag object is created every time a tag is discovered (comes into range), even if it is the same physical tag. If a tag is removed and then returned into range, then only the most recent tag object can be successfully used to create a  `[TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)`.

### Tag Dispatch

When a tag is discovered, a `[Tag](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA)` object is created and passed to a single activity via the `[NfcAdapter.EXTRA_TAG](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#EXTRA_TAG)` extra in an `[Intent](https://developer.android.com/reference/android/content/Intent.html?hl=CA)` via `[Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent))`. A four stage dispatch is used to select the most appropriate activity to handle the tag. The Android OS executes each stage in order, and completes dispatch as soon as a single matching activity is found. If there are multiple matching activities found at any one stage then the Android activity chooser dialog is shown to allow the user to select the activity to receive the tag.

The Tag dispatch mechanism was designed to give a high probability of dispatching a tag to the correct activity without showing the user an activity chooser dialog. This is important for NFC interactions because they are very transient -- if a user has to move the Android device to choose an application then the connection will likely be broken.

#### 1. Foreground activity dispatch

A foreground activity that has called `[NfcAdapter.enableForegroundDispatch()](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#enableForegroundDispatch(android.app.Activity,%20android.app.PendingIntent,%20android.content.IntentFilter[],%20java.lang.String[][]))` is given priority. See the documentation on `[NfcAdapter.enableForegroundDispatch()](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#enableForegroundDispatch(android.app.Activity,%20android.app.PendingIntent,%20android.content.IntentFilter[],%20java.lang.String[][]))`for its usage.

#### 2. NDEF data dispatch

If the tag contains NDEF data the system inspects the first `[NdefRecord](https://developer.android.com/reference/android/nfc/NdefRecord.html?hl=CA)` in the first`[NdefMessage](https://developer.android.com/reference/android/nfc/NdefMessage.html?hl=CA)`. If the record is a URI, SmartPoster, or MIME data`[Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent))` is called with `[NfcAdapter.ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_NDEF_DISCOVERED)`. For URI and SmartPoster records the URI is put into the intent's data field. For MIME records the MIME type is put in the intent's type field. This allows activities to register to be launched only when data they know how to handle is present on a tag. This is the preferred method of handling data on a tag since NDEF data can be stored on many types of tags and doesn't depend on a specific tag technology. See `[NfcAdapter.ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_NDEF_DISCOVERED)` for more detail. If the tag does not contain NDEF data, or if no activity is registered for `[NfcAdapter.ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_NDEF_DISCOVERED)` with a matching data URI or MIME type then dispatch moves to stage 3.

#### 3. Tag Technology dispatch

`[Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent))` is called with `[NfcAdapter.ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TECH_DISCOVERED)` to dispatch the tag to an activity that can handle the technologies present on the tag. Technologies are defined as sub-classes of `[TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)`, see the package `[android.nfc.tech](https://developer.android.com/reference/android/nfc/tech/package-summary.html?hl=CA)`. The Android OS looks for an activity that can handle one or more technologies in the tag. See `[NfcAdapter.ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TECH_DISCOVERED)` for more detail.

#### 4. Fall-back dispatch

If no activity has been matched then `[Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent))` is called with`[NfcAdapter.ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TAG_DISCOVERED)`. This is intended as a fall-back mechanism. See `[NfcAdapter.ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TAG_DISCOVERED)`.

### NFC Tag Background

An NFC tag is a passive NFC device, powered by the NFC field of this Android device while it is in range. Tag's can come in many forms, such as stickers, cards, key fobs, or even embedded in a more sophisticated device.

Tags can have a wide range of capabilities. Simple tags just offer read/write semantics, and contain some one time programmable areas to make read-only. More complex tags offer math operations and per-sector access control and authentication. The most sophisticated tags contain operating environments allowing complex interactions with the code executing on the tag. Use  `[TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)`  classes to access a broad range of capabilities available in NFC tags.








### writeToParcel

added in  [API level 10](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html?hl=CA#ApiLevels)

public void writeToParcel ([Parcel](https://developer.android.com/reference/android/os/Parcel.html?hl=CA) dest,
                int flags)

Flatten this object in to a Parcel.





### getTechList

added in  [API level 10](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html?hl=CA#ApiLevels)

public [String[]](https://developer.android.com/reference/java/lang/String.html?hl=CA) getTechList ()

Get the technologies available in this tag, as fully qualified class names.

A technology is an implementation of the  `[TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)`  interface, and can be instantiated by calling the static  `get(Tag)`  method on the implementation with this Tag. The  `[TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)`  object can then be used to perform advanced, technology-specific operations on a tag.

Android defines a mandatory set of technologies that must be correctly enumerated by all Android NFC devices, and an optional set of proprietary technologies. See  `[TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)`  for more details.

The ordering of the returned array is undefined and should not be relied upon.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQyOTc2NDg0XX0=
-->