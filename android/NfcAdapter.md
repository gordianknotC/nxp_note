
# Near field communication overview


Android-powered devices with NFC simultaneously support three main modes of operation:

1.  **Reader/writer mode**, allowing the NFC device to read and/or write passive NFC tags and stickers.
2.  **P2P mode**, allowing the NFC device to exchange data with other NFC peers; this operation mode is used by Android Beam.
3.  **Card emulation mode**, allowing the NFC device itself to act as an NFC card. The emulated NFC card can then be accessed by an external NFC reader, such as an NFC point-of-sale terminal.


# NFC basics

This document describes the basic NFC tasks you perform in Android. It explains how to send and receive NFC data in the form of NDEF messages and describes the Android framework APIs that support these features. For more advanced topics, including a discussion of working with non-NDEF data, see  [Advanced NFC](https://developer.android.com/guide/topics/connectivity/nfc/advanced-nfc.html).

There are two major use cases when working with NDEF data and Android:

-   Reading NDEF data from an NFC tag
-   Beaming NDEF messages from one device to another with  [Android Beam™](https://developer.android.com/guide/topics/connectivity/nfc/nfc#p2p)

Reading NDEF data from an NFC tag is handled with the [tag dispatch system](https://developer.android.com/guide/topics/connectivity/nfc/nfc#tag-dispatch), which analyzes discovered NFC tags, appropriately categorizes the data, and starts an application that is interested in the categorized data. An application that wants to handle the scanned NFC tag can [declare an intent filter](https://developer.android.com/guide/topics/connectivity/nfc/nfc#filtering-intents) and request to handle the data.

The Android Beam™ feature allows a device to push an NDEF message onto another device by physically tapping the devices together. This interaction provides an easier way to send data than other wireless technologies like Bluetooth, because with NFC, no manual device discovery or pairing is required.The connection is automatically started when two devices come into range.

## The tag dispatch system

Android-powered devices are usually looking for NFC tags when the screen is unlocked, unless NFC is disabled in the device's Settings menu. 

>When an Android-powered device discovers an NFC tag, the desired behavior is to have the most appropriate activity handle the intent **without asking the user** what application to use. Because devices scan NFC tags at a very short range, it is likely that making users manually select an activity would force them to move the device away from the tag and break the connection.

 _You should develop your activity to only handle the NFC tags that your activity cares about to prevent the Activity Chooser from appearing._


**Process of the tag dispatch system**

1.  Parsing the NFC tag and figuring out the MIME type or a URI that identifies the data payload in the tag.
2.  Encapsulating the MIME type or URI and the payload into an intent. These first two steps are described in  [How NFC tags are mapped to MIME types and URIs](https://developer.android.com/guide/topics/connectivity/nfc/nfc#ndef).
3.  Starts an activity based on the intent. This is described in  [How NFC Tags are Dispatched to Applications](https://developer.android.com/guide/topics/connectivity/nfc/nfc#dispatching).


-----------------

### How NFC tags are dispatched to applications

When the tag dispatch system is done creating an intent that encapsulates the NFC tag and its identifying information, it sends the intent to an interested application that filters for the intent. 
> ⚡ If more than one application can handle the intent, the Activity Chooser is presented so the user can select the Activity. 

The tag dispatch system defines three intents, which are **listed in order of highest** to lowest priority:

|||
|--|--|
|  |  |

1.  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED): This intent is used to **start an Activity** when a tag that contains an **NDEF payload is scanned** and is of a **recognized** type. This is the highest priority intent, and the tag dispatch system tries to start an Activity with this intent before any other intent, whenever possible.
3.  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED): If no activities register to handle the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intent, the tag dispatch system tries to start an application with this intent. This intent is also directly started (without starting  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)`  first) if the tag that is scanned contains NDEF data that cannot be mapped to a MIME type or URI, or if the tag does not contain NDEF data but is of a known tag technology.
4.  [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED): This intent is started if no activities handle the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intents.

The basic way the tag dispatch system works is as follows:

1.  Try to start an Activity with the intent that was created by the tag dispatch system when parsing the NFC tag (either  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)`  or  `[ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)).
2.  If no activities filter for that intent, try to start an Activity with the next lowest priority intent (either  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  or  [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED)) until an application filters for the intent or until the tag dispatch system tries all possible intents.
3.  If no applications filter for any of the intents, do nothing.

![](https://developer.android.com/images/nfc_tag_dispatch.png)

**Figure 1.** Tag Dispatch System

Whenever possible, work with NDEF messages and the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intent, because it is the most specific out of the three. This intent allows you to start your application at a more appropriate time than the other two intents, giving the user a better experience.






<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NjU5MDE0NDEsOTMxNDYyMzM0LC05Mz
YyMTc3MDJdfQ==
-->