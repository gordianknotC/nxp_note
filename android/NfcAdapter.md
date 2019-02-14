
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
| 1.  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED): | This intent is used to **start an Activity** when a tag that contains an **NDEF payload is scanned** and is of a **recognized** type. This is the highest priority intent, and the tag dispatch system tries to start an Activity with this intent before any other intent, whenever possible. |
| 3.  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED): | **If no** activities register to handle the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intent, the tag dispatch system tries to start an application with this intent. This intent is also directly started (without starting [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  first) if the tag that is scanned **contains** NDEF data that **cannot be mapped to a MIME type** or URI, or if the tag **does not** contain **NDEF** data but is of a **known** tag technology. |
| [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED):| This intent is started if no activities handle the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intents.|
| -- | -- |
 


The basic way the tag dispatch system works is as follows:

1.  Try to start an Activity with the intent that was created by the tag dispatch system when parsing the NFC tag (either  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)`  or  `[ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)).
2.  If no activities filter for that intent, try to start an Activity with the next lowest priority intent (either  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  or  [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED)) until an application filters for the intent or until the tag dispatch system tries all possible intents.
3.  If no applications filter for any of the intents, do nothing.

![](https://developer.android.com/images/nfc_tag_dispatch.png)

**Figure 1.** Tag Dispatch System

Whenever possible, work with NDEF messages and the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intent, because it is the most specific out of the three. This intent allows you to start your application at a more appropriate time than the other two intents, giving the user a better experience.



## Request NFC access in the Android manifest

Before you can access a device's NFC hardware and properly handle NFC intents, declare these items in your  `AndroidManifest.xml`  file:

-   The NFC  **`<uses-permission>`**  element to access the NFC hardware:
	``` xml
	<uses-permission  android:name="android.permission.NFC"  />
	```    
-   The minimum SDK version that your application can support.  API level 14 
    ```xml
    <uses-sdk android:minSdkVersion="10"/>
    ```
-   The  **`uses-feature`**  element so that your application shows up in Google Play only for devices that **have NFC** hardware:
	```xml
	<uses-feature  android:name="android.hardware.nfc"  android:required="true"  />
	```
    If your application uses NFC functionality, but that functionality is not crucial to your application, you can omit the  `uses-feature`  element and check for NFC avalailbility at runtime by checking to see if  [getDefaultAdapter()](https://developer.android.com/reference/android/nfc/NfcAdapter.html#getDefaultAdapter(android.content.Context))  is  `null`.
    

## Filter for NFC intents

To start your application when an NFC tag that you want to handle is scanned, your application can filter for one, two, or all three of the NFC intents in the Android manifest. However, you usually want to filter for the  [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intent for the most control of when your application starts. The  [TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intent is a fallback for  [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  when no applications filter for [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or for when the payload is not NDEF. Filtering for  [TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED)  is usually too general of a category to filter on. Many applications will filter for  [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or  [TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED) before  [TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED), so your application has a low probability of starting.  [TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED) is only available as a last resort for applications to filter for in the cases where no other applications are installed to handle the [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or  [TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intent.

Because **NFC tag deployments vary** and are many times **not under your control,** this is not always possible, **`which is why you can fallback to the other two intents when necessary.`** When you have control over the types of tags and data written, it is recommended that you use NDEF to format your tags. The following sections describe how to filter for each type of intent.


.

## Filter Each Type of Intent
### ACTION_NDEF_DISCOVERED

To filter for  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intents, declare the intent filter along with the type of data that you want to filter for. The following example filters for  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intents with a MIME type of  `text/plain`:
```xml
<intent-filter>  
	<action  android:name="android.nfc.action.NDEF_DISCOVERED"/>  	
	<category  android:name="android.intent.category.DEFAULT"/>  
	<data  android:mimeType="text/plain"  />  
</intent-filter>
```
The following example filters for a URI in the form of`http://developer.android.com/index.html`.
```xml
<intent-filter>  
	<action  android:name="android.nfc.action.NDEF_DISCOVERED"/>  
	<category  android:name="android.intent.category.DEFAULT"/>  
	<data  android:scheme="http"  
		android:host="developer.android.com"  
		android:pathPrefix="/index.html"  />  
</intent-filter>
```



------------------------------------

### ACTION_TECH_DISCOVERED

If your activity filters for the  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intent, you must create an XML resource file that specifies the technologies that your activity supports within a  `tech-list`  set. Your activity is considered a match if a  `tech-list`  set is a subset of the technologies that are supported by the tag, which you can obtain by calling  [getTechList()](https://developer.android.com/reference/android/nfc/Tag.html#getTechList()).

For example, if the tag that is scanned supports MifareClassic, NdefFormatable, and NfcA, your  `tech-list`  set must specify all three, two, or one of the technologies (and nothing else) in order for your activity to be matched.

The following sample defines all of the technologies. You can remove the ones that you do not need. Save this file (you can name it anything you wish) in the  `<project-root>/res/xml`  folder.
```xml
<resources  xmlns:xliff="urn:oasis:names:tc:xliff:document:1.2">  
<tech-list>  
	<tech>android.nfc.tech.IsoDep</tech>  
	<tech>android.nfc.tech.NfcA</tech>  
	<tech>android.nfc.tech.NfcB</tech>  
	<tech>android.nfc.tech.NfcF</tech>  
	<tech>android.nfc.tech.NfcV</tech>  
	<tech>android.nfc.tech.Ndef</tech>  
	<tech>android.nfc.tech.NdefFormatable</tech>  
	<tech>android.nfc.tech.MifareClassic</tech>  
	<tech>android.nfc.tech.MifareUltralight</tech>  
</tech-list>  
</resources>
```
You can also specify multiple  `tech-list`  sets. Each of the  `tech-list`  sets is considered independently, and your activity is considered a match if any single  `tech-list`  set is a subset of the technologies that are returned by  [getTechList()](https://developer.android.com/reference/android/nfc/Tag.html#getTechList()). This provides  **`AND`**  and  **`OR`**  semantics for matching technologies. 

The following example matches tags that can support the **NfcA and Ndef** technologies or can support the **NfcB and Ndef** technologies:
```xml
<resources  xmlns:xliff="urn:oasis:names:tc:xliff:document:1.2">  
	<tech-list>  
		<tech>android.nfc.tech.NfcA</tech>  
		<tech>android.nfc.tech.Ndef</tech>  
	</tech-list>  
</resources>  
```
```xml
<resources  xmlns:xliff="urn:oasis:names:tc:xliff:document:1.2">  
	<tech-list>  
		<tech>android.nfc.tech.NfcB</tech>  
		<tech>android.nfc.tech.Ndef</tech>  
	</tech-list>  
</resources>
```
In your  `AndroidManifest.xml`  file, specify the resource file that you just created in the  `<meta-data>`  element inside the  `<activity>`  element like in the following example:
```xml
<activity>  
	...  
	<intent-filter>  
		<action  android:name="android.nfc.action.TECH_DISCOVERED"/>  
	</intent-filter>  
	  
	<meta-data  android:name="android.nfc.action.TECH_DISCOVERED"  
		android:resource="@xml/nfc_tech_filter"  />  
	...  
</activity>
```
For more information about working with tag technologies and the  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intent, see  [Working with Supported Tag Technologies](https://developer.android.com/guide/topics/connectivity/nfc/advanced-nfc.html#tag-tech)  in the Advanced NFC document.



------------------------------------

### ACTION_TAG_DISCOVERED

To filter for  [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED)  use the following intent filter:
```xml
<intent-filter>  
	<action  android:name="android.nfc.action.TAG_DISCOVERED"/>  
</intent-filter>
```
-----------------
.

### ⚡Obtain information from intents

If an activity starts because of an NFC intent, you can obtain information about the scanned NFC tag from the intent. Intents can contain the following extras depending on the tag that was scanned:

-   [EXTRA_TAG](https://developer.android.com/reference/android/nfc/NfcAdapter.html#EXTRA_TAG)  (required): A  [Tag](https://developer.android.com/reference/android/nfc/Tag.html)  object representing the scanned tag.
-    [EXTRA_NDEF_MESSAGES](https://developer.android.com/reference/android/nfc/NfcAdapter.html#EXTRA_NDEF_MESSAGES)   (optional): An array of NDEF messages parsed from the tag. This extra is mandatory on   [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)   intents.
-    [EXTRA_ID](https://developer.android.com/reference/android/nfc/NfcAdapter.html#EXTRA_ID)   (optional): The low-level ID of the tag.

To obtain these extras, check to see if your activity was launched with one of the NFC intents to ensure that a tag was scanned, and then obtain the extras out of the intent. The following example checks for the   [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)   intent and gets the NDEF messages from an intent extra.

```kotlin
override  fun onNewIntent(intent:  Intent)  {  
	super.onNewIntent(intent)  
	...  
	if  (NfcAdapter.ACTION_NDEF_DISCOVERED == intent.action)  { 
		intent.getParcelableArrayExtra(NfcAdapter.EXTRA_NDEF_MESSAGES)
		?.also { 
		rawMessages ->  
			val messages:  List<NdefMessage>  = 
				rawMessages.map { it as  NdefMessage  }  
			// Process the messages array.  ...  
		}  
	}  
}
```

Alternatively, you can obtain a [Tag](https://developer.android.com/reference/android/nfc/Tag.html) object from the intent, which will contain the payload and allow you to enumerate the tag's technologies:
```kotlin
val tag: Tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG)
```









<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAwMzMyMzQ4OSwtMjgzMTQ5NjgsLTQyNT
MwNTc5MCwxMTIzOTc2NjkzLC0xMTM3ODg3NTUxLC03ODA5Mjc1
MjEsLTIyNTg5NjI2NSw5MzE0NjIzMzQsLTkzNjIxNzcwMl19
-->