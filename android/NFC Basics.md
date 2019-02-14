
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


.


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

> 📘 **Note** on _AAR only supported at application level_
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


## ⚡Use the foreground dispatch system

The foreground dispatch system **`allows an activity to intercept an intent and claim priority`** over other activities that handle the same intent. Using this system involves constructing a few data structures for the Android system to be able to send the appropriate intents to your application. To enable the foreground dispatch system:

Add the following code in the `onCreate()` method of your activity:

1.  Create a  [PendingIntent](https://developer.android.com/reference/android/app/PendingIntent.html)` object so the Android system can populate it with the details of the tag when it is scanned.
	- Create a [PendingIntent](https://developer.android.com/reference/android/app/PendingIntent.html) object so the Android system can populate it with the details of the tag when it is scanned.
	```kotlin
		val intent = Intent(this, javaClass).apply {
		    addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP)
		}
		var pendingIntent: PendingIntent = 
			PendingIntent.getActivity(this, 0, intent, 0)
	```

	- Declare intent filters to handle the intents that you want to intercept. The foreground dispatch system checks the specified intent filters with the intent that is received **when the device scans a tag**. 
		> If it matches, then your application handles the intent. 
		> If it does not match, the foreground dispatch system falls back to the intent dispatch system. 
	- Specifying a `null` array of intent filters and technology filters, specifies that you want to filter for all tags that fallback to the `TAG_DISCOVERED` intent. The code snippet below handles all MIME types for `NDEF_DISCOVERED`. You should only handle the ones that you need.
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



.

----------------------------------------------------


### enableForegroundDispatch

added in  [API level 10](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)

```java
public void enableForegroundDispatch (
	Activity activity,
	PendingIntent intent,
	IntentFilter[] filters,
	String[][] techLists)
```

Enable foreground dispatch to the given Activity.

This will give give priority to the foreground activity when dispatching a discovered  [Tag](https://developer.android.com/reference/android/nfc/Tag.html)  to an application.

If any IntentFilters are provided to this method they are used to match dispatch Intents for both the  [ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  and  [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED). Since  [ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  relies on meta data outside of the IntentFilter matching for that dispatch Intent is handled by passing in the tech lists separately. Each first level entry in the tech list represents an array of technologies that must all be present to match. If any of the first level sets match then the dispatch is routed through the given PendingIntent. In other words, the second level is ANDed together and the first level entries are ORed together.

If you pass  `null`  for both the  `filters`  and  `techLists`  parameters that acts a wild card and will cause the foreground activity to receive all tags via the  [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED)  intent.

This method must be called from the main thread, and only when the activity is in the foreground (resumed). Also, activities must call  [disableForegroundDispatch(Activity)](https://developer.android.com/reference/android/nfc/NfcAdapter.html#disableForegroundDispatch(android.app.Activity))  before the completion of their  [Activity.onPause()](https://developer.android.com/reference/android/app/Activity.html#onPause())  callback to disable foreground dispatch after it has been enabled.

Requires the  [Manifest.permission.NFC](https://developer.android.com/reference/android/Manifest.permission.html#NFC)  permission.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NDg3MDM2MzgsLTEwMjQyNDIwNjZdfQ
==
-->