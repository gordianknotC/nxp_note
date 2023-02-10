NPX learning notes

2018 android nfc(npx/flutter) 專案學習筆記

__Table of Index__
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Android](#android)
  - [NFC Basics:](#nfc-basics)
    - [Near field communication overview](#near-field-communication-overview)
    - [NFC basics](#nfc-basics)
    - [Filter for NFC intents](#filter-for-nfc-intents)
    - [Filter Each Type of Intent](#filter-each-type-of-intent)
    - [ACTION_TECH_DISCOVERED](#action_tech_discovered)
    - [ACTION_TAG_DISCOVERED](#action_tag_discovered)
    - [⚡Obtain information from intents](#obtain-information-from-intents)
    - [Create common types of NDEF records](#create-common-types-of-ndef-records)
    - [Android Application Reccords (AAR)](#android-application-reccords-aar)
    - [⚡Use the foreground dispatch system](#use-the-foreground-dispatch-system)
  - [NFC Tag:](#nfc-tag)
  - [Tag](#tag)
    - [Tag Dispatch](#tag-dispatch)
    - [getId](#getid)
    - [writeToParcel](#writetoparcel)
    - [getTechList](#gettechlist)
  - [Ndef Record:](#ndef-record)
    - [Create common types of NDEF records](#create-common-types-of-ndef-records-1)
    - [⚡Use the foreground dispatch system](#use-the-foreground-dispatch-system-1)
  - [Trouble Shoot - A](#trouble-shoot---a)
    - [Error:Program type already present:](#errorprogram-type-already-present)
    - [INSTALL_FAILED_VERSION_DOWNGRADE](#install_failed_version_downgrade)
    - [Failed linking file resources | failed processing manifest error](#failed-linking-file-resources--failed-processing-manifest-error)
    - [Cannot read packageName from path/to/AndroidManifest.xml](#cannot-read-packagename-from-pathtoandroidmanifestxml)
    - [AndroidManifest.xml: Error: 'A' is not a valid file-based resource name character](#androidmanifestxml-error-a-is-not-a-valid-file-based-resource-name-character)
    - [strings_en.arb: Error: The file name must end with .xml](#strings_enarb-error-the-file-name-must-end-with-xml)
- [Aqueduct](#aqueduct)
  - [Auth 2.0:](#auth-20)
  - [Overview](#overview)
    - [Tasks](#tasks)
  - [What is OAuth 2.0?](#what-is-oauth-20)
  - [Auth Code Controller:](#auth-code-controller)
  - [Issue Authorization Codes with AuthCodeController](#issue-authorization-codes-with-authcodecontroller)
  - [Auth Controller:](#auth-controller)
  - [Issue Access Tokens with AuthController](#issue-access-tokens-with-authcontroller)
  - [CRUD:](#crud)
  - [Inserting, Updating, Deleting and Fetching Objects](#inserting-updating-deleting-and-fetching-objects)
    - [Specifying Result Properties](#specifying-result-properties)
  - [Security Route:](#security-route)
  - [Securing Routes with Authorizer](#securing-routes-with-authorizer)
  - [Setup Auth:](#setup-auth)
  - [Creating AuthServers to Authenticate and Authorize](#creating-authservers-to-authenticate-and-authorize)
    - [Creating Instances of AuthServer and AuthServerDelegate](#creating-instances-of-authserver-and-authserverdelegate)
    - [Using ManagedAuthDelegate](#using-managedauthdelegate)
  - [Understand Authentication:](#understand-authentication)
    - [Understanding Login Authentication](#understanding-login-authentication)
  - [Codecs:](#codecs)
    - [Streaming Response Bodies](#streaming-response-bodies)
  - [Codecs and Content Types](#codecs-and-content-types)
    - [register codecs into CodeRegistery](#register-codecs-into-coderegistery)
    - [Compression with gzip](#compression-with-gzip)
  - [Serializable Objects](#serializable-objects)
    - [Serializable and OpenAPI Generation](#serializable-and-openapi-generation)
  - [🔥 Example: multipart/form-data](#-example-multipartform-data)
  - [Processing MIME multipart media types  #](#processing-mime-multipart-media-types--)
  - [Core Concept:](#core-concept)
  - [Core Concepts](#core-concepts)
    - [Resources](#resources)
    - [Routing](#routing)
    - [Controllers](#controllers)
    - [Request Binding @bind](#request-binding-bind)
    - [Using ManagedContext to Connect to a Database](#using-managedcontext-to-connect-to-a-database)
    - [Configuration Files](#configuration-files)
  - [DataBase:](#database)
  - [Example: One-to-Many Relationship](#example-one-to-many-relationship)
    - [Example: Hierarchical Relationships (Self Referencing)](#example-hierarchical-relationships-self-referencing)
  - [Example: Many-to-Many Relationship](#example-many-to-many-relationship)
  - [ManagedObject Serialization and Deserialization](#managedobject-serialization-and-deserialization)
  - [Basic Behavior](#basic-behavior)
  - [Behavior of Null Values](#behavior-of-null-values)
  - [Behavior of Transient Properties](#behavior-of-transient-properties)
  - [Auth](#auth)
  - [JSON:](#json)
  - [JSON Document Storage](#json-document-storage)
    - [JSON Columns in Relational Databases](#json-columns-in-relational-databases)
    - [The Document Data Type](#the-document-data-type)
    - [Basic Operations on Document Properties](#basic-operations-on-document-properties)
    - [Fetching Sub-documents](#fetching-sub-documents)
  - [Multipart:](#multipart)
    - [Multipart header](#multipart-header)
  - [Resource Controller:](#resource-controller)
  - [ResourceController](#resourcecontroller)
    - [Operation Methods](#operation-methods)
    - [Routing to a ResourceController](#routing-to-a-resourcecontroller)
    - [Request Bindings](#request-bindings)
    - [Other ResourceController Behavior](#other-resourcecontroller-behavior)
  - [Serving Files:](#serving-files)
  - [Serving Files and Caching](#serving-files-and-caching)
    - [FileController](#filecontroller)
    - [Caching](#caching)
    - [File Serving and Caching Outside of FileController](#file-serving-and-caching-outside-of-filecontroller)
  - [Validation:](#validation)
  - [Validating Data](#validating-data)
- [Flutter](#flutter)
  - [addActivityResultListener](#addactivityresultlistener)
  - [Flutter/Android 專案目錄結構說明](#flutterandroid-%E5%B0%88%E6%A1%88%E7%9B%AE%E9%8C%84%E7%B5%90%E6%A7%8B%E8%AA%AA%E6%98%8E)
    - [說明Android Project wrapper for flutter](#%E8%AA%AA%E6%98%8Eandroid-project-wrapper-for-flutter)
    - [未完成](#%E6%9C%AA%E5%AE%8C%E6%88%90)
  - [addNewIntentListener](#addnewintentlistener)
    - [Sample Code](#sample-code)
  - [To Elaborate](#to-elaborate)
    - [BroadCastReceiver Overview](#broadcastreceiver-overview)
    - [BroadCastReceiver examples](#broadcastreceiver-examples)
    - [LocalBroadCastManager](#localbroadcastmanager)
    - [addNewIntentListener](#addnewintentlistener-1)
  - [Channel android to flutter](#channel-android-to-flutter)
    - [Hiddable SliverAppbar](#hiddable-sliverappbar)
    - [TabBar Nested in SliverAppBar](#tabbar-nested-in-sliverappbar)
    - [Handy Tool for Preventing Rebuild](#handy-tool-for-preventing-rebuild)
  - [Trouble Shooting](#trouble-shooting)
    - [Flutter Constaantly Rebuild](#flutter-constaantly-rebuild)
    - [Rendering Problems](#rendering-problems)
- [NXP](#nxp)
  - [Authenticate And Protect](#authenticate-and-protect)
    - [Authenticate](#authenticate)
    - [Protect](#protect)
    - [Unprotect](#unprotect)
  - [How Ntag I2C Demo / Tag be Initialized](#how-ntag-i2c-demo--tag-be-initialized)
  - [Handling null Tag](#handling-null-tag)
  - [Initialize Ntag_IC2_Command](#initialize-ntag_ic2_command)
  - [Initialize MinimalNtag_IC2_Command](#initialize-minimalntag_ic2_command)
  - [Ntag Demo MainActivity](#ntag-demo-mainactivity)
    - [onCreate](#oncreate)
    - [onPause](#onpause)
    - [onActivityResult](#onactivityresult)
    - [onNewIntent](#onnewintent)
    - [launchDemo](#launchdemo)
    - [launchNdefDemo](#launchndefdemo)
  - [NtagDemo - NdefFragment](#ntagdemo---ndeffragment)
    - [onCreate](#oncreate-1)
    - [enableNdefReadLoop](#enablendefreadloop)
    - [readNdefClick](#readndefclick)
    - [writeNdefClick](#writendefclick)
  - [Ntag_I2C_Demo](#ntag_i2c_demo)
    - [Properties and Constructor](#properties-and-constructor)
  - [Constructor](#constructor)
    - [isConnected](#isconnected)
    - [isTagPresent](#istagpresent)
    - [isReady](#isready)
    - [getProduct](#getproduct)
    - [setBoardVersion](#setboardversion)
    - [resetTagMemory](#resettagmemory)
    - [getRegister_Settings](#getregister_settings)
    - [resetSessionRegisters](#resetsessionregisters)
  - [Ntag_I2C_Demo - How New Intent be Processed](#ntag_i2c_demo---how-new-intent-be-processed)
  - [Ntag_I2C_Demo - mermaid](#ntag_i2c_demo---mermaid)
    - [NDEF()](#ndef)
    - [NDEFReadTask()](#ndefreadtask)
    - [NDEFWrite](#ndefwrite)
    - [NDEFReadFinish](#ndefreadfinish)
    - [finishAllTasks](#finishalltasks)
    - [resetTagMemory](#resettagmemory-1)
    - [readSessionRegisters](#readsessionregisters)
    - [readWriteConfigRegister](#readwriteconfigregister)
    - [writeConfigRegisters](#writeconfigregisters)
    - [readTagContent](#readtagcontent)
    - [resetTagContent](#resettagcontent)
    - [LED](#led)
    - [LEDFinish](#ledfinish)
    - [ObtainAuthStatus()](#obtainauthstatus)
    - [Auth()](#auth)
    - [SRAMSpeedtest | SRAMSpeedtestTask (AsyncTask)](#sramspeedtest--sramspeedtesttask-asynctask)
    - [createNdefTextMessage](#createndeftextmessage)
    - [createNdefUriMessage](#createndefurimessage)
    - [createNdefBSSPMessage](#createndefbsspmessage)
    - [createNdefSpMessage](#createndefspmessage)
  - [Ntag_I2C_Demo - How Demo be Initialized](#ntag_i2c_demo---how-demo-be-initialized)
  - [How ReadEEPROM implements](#how-readeeprom-implements)
    - [in Ntag_I2C_Command](#in-ntag_i2c_command)
    - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command)
  - [How ReadNDEF Implements](#how-readndef-implements)
    - [in Ntag_I2C_Command](#in-ntag_i2c_command-1)
    - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-1)
  - [Reading Product Information](#reading-product-information)
    - [Ntag_I2C](#ntag_i2c)
    - [MinimalNtag_I2C](#minimalntag_i2c)
  - [Reading configRegisters/sessionRegisters/...](#reading-configregisterssessionregisters)
    - [in Ntag_I2C_Command](#in-ntag_i2c_command-2)
    - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-2)
    - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-3)
    - [difference bewteen MinimalNtag and Ntag](#difference-bewteen-minimalntag-and-ntag)
    - [SR_Offset](#sr_offset)
  - [How readSRAM implements](#how-readsram-implements)
  - [How MFU(MifareUltralight) / NFCA transceive Command](#how-mfumifareultralight--nfca-transceive-command)
  - [How transceive operates](#how-transceive-operates)
  - [Wait for Read and Write](#wait-for-read-and-write)
  - [How to Write EEPROM](#how-to-write-eeprom)
    - [in Ntag_I2C_Command](#in-ntag_i2c_command-3)
    - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-4)
    - [example on writeEEPROM listener](#example-on-writeeeprom-listener)
    - [Difference Between Ntag and Minimal Ntag](#difference-between-ntag-and-minimal-ntag)
  - [How WriteNDEF Implements](#how-writendef-implements)
    - [in Ntag_I2C_Command](#in-ntag_i2c_command-4)
    - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-5)
    - [writeEEPROM listener](#writeeeprom-listener)
    - [CreateNDEFMessage](#createndefmessage)
  - [How Write Auth | Config Registers Imlements](#how-write-auth--config-registers-imlements)
    - [Write Auth Registers](#write-auth-registers)
    - [Write Config Registers](#write-config-registers)
  - [How Write SRAM Implements](#how-write-sram-implements)
    - [in Ntag_I2C_Command](#in-ntag_i2c_command-5)
    - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->










# Android
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [NFC Basics:](#nfc-basics)
  - [Near field communication overview](#near-field-communication-overview)
  - [NFC basics](#nfc-basics)
  - [Filter for NFC intents](#filter-for-nfc-intents)
  - [Filter Each Type of Intent](#filter-each-type-of-intent)
  - [ACTION_TECH_DISCOVERED](#action_tech_discovered)
  - [ACTION_TAG_DISCOVERED](#action_tag_discovered)
  - [⚡Obtain information from intents](#obtain-information-from-intents)
  - [Create common types of NDEF records](#create-common-types-of-ndef-records)
  - [Android Application Reccords (AAR)](#android-application-reccords-aar)
  - [⚡Use the foreground dispatch system](#use-the-foreground-dispatch-system)
- [NFC Tag:](#nfc-tag)
- [Tag](#tag)
  - [Tag Dispatch](#tag-dispatch)
  - [getId](#getid)
  - [writeToParcel](#writetoparcel)
  - [getTechList](#gettechlist)
- [Ndef Record:](#ndef-record)
  - [Create common types of NDEF records](#create-common-types-of-ndef-records-1)
  - [⚡Use the foreground dispatch system](#use-the-foreground-dispatch-system-1)
- [Trouble Shoot - A](#trouble-shoot---a)
  - [Error:Program type already present:](#errorprogram-type-already-present)
  - [INSTALL_FAILED_VERSION_DOWNGRADE](#install_failed_version_downgrade)
  - [Failed linking file resources | failed processing manifest error](#failed-linking-file-resources--failed-processing-manifest-error)
  - [Cannot read packageName from path/to/AndroidManifest.xml](#cannot-read-packagename-from-pathtoandroidmanifestxml)
  - [AndroidManifest.xml: Error: 'A' is not a valid file-based resource name character](#androidmanifestxml-error-a-is-not-a-valid-file-based-resource-name-character)
  - [strings_en.arb: Error: The file name must end with .xml](#strings_enarb-error-the-file-name-must-end-with-xml)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->




## NFC Basics:

### Near field communication overview


Android-powered devices with NFC simultaneously support three main modes of operation:

1.  **Reader/writer mode**, allowing the NFC device to read and/or write passive NFC tags and stickers.
2.  **P2P mode**, allowing the NFC device to exchange data with other NFC peers; this operation mode is used by Android Beam.
3.  **Card emulation mode**, allowing the NFC device itself to act as an NFC card. The emulated NFC card can then be accessed by an external NFC reader, such as an NFC point-of-sale terminal.


### NFC basics

This document describes the basic NFC tasks you perform in Android. It explains how to send and receive NFC data in the form of NDEF messages and describes the Android framework APIs that support these features. For more advanced topics, including a discussion of working with non-NDEF data, see  [Advanced NFC](https://developer.android.com/guide/topics/connectivity/nfc/advanced-nfc.html).

There are two major use cases when working with NDEF data and Android:

-   Reading NDEF data from an NFC tag
-   Beaming NDEF messages from one device to another with  [Android Beam™](https://developer.android.com/guide/topics/connectivity/nfc/nfc#p2p)

Reading NDEF data from an NFC tag is handled with the [tag dispatch system](https://developer.android.com/guide/topics/connectivity/nfc/nfc#tag-dispatch), which analyzes discovered NFC tags, appropriately categorizes the data, and starts an application that is interested in the categorized data. An application that wants to handle the scanned NFC tag can [declare an intent filter](https://developer.android.com/guide/topics/connectivity/nfc/nfc#filtering-intents) and request to handle the data.

The Android Beam™ feature allows a device to push an NDEF message onto another device by physically tapping the devices together. This interaction provides an easier way to send data than other wireless technologies like Bluetooth, because with NFC, no manual device discovery or pairing is required.The connection is automatically started when two devices come into range.

#### The tag dispatch system

Android-powered devices are usually looking for NFC tags when the screen is unlocked, unless NFC is disabled in the device's Settings menu. 

>When an Android-powered device discovers an NFC tag, the desired behavior is to have the most appropriate activity handle the intent **without asking the user** what application to use. Because devices scan NFC tags at a very short range, it is likely that making users manually select an activity would force them to move the device away from the tag and break the connection.

 _You should develop your activity to only handle the NFC tags that your activity cares about to prevent the Activity Chooser from appearing._


**Process of the tag dispatch system**

1.  Parsing the NFC tag and figuring out the MIME type or a URI that identifies the data payload in the tag.
2.  Encapsulating the MIME type or URI and the payload into an intent. These first two steps are described in  [How NFC tags are mapped to MIME types and URIs](https://developer.android.com/guide/topics/connectivity/nfc/nfc#ndef).
3.  Starts an activity based on the intent. This is described in  [How NFC Tags are Dispatched to Applications](https://developer.android.com/guide/topics/connectivity/nfc/nfc#dispatching).


-----------------

#### How NFC tags are dispatched to applications

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



#### Request NFC access in the Android manifest

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
    

### Filter for NFC intents

To start your application when an NFC tag that you want to handle is scanned, your application can filter for one, two, or all three of the NFC intents in the Android manifest. However, you usually want to filter for the  [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  intent for the most control of when your application starts. The  [TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intent is a fallback for  [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  when no applications filter for [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or for when the payload is not NDEF. Filtering for  [TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED)  is usually too general of a category to filter on. Many applications will filter for  [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or  [TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED) before  [TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED), so your application has a low probability of starting.  [TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED) is only available as a last resort for applications to filter for in the cases where no other applications are installed to handle the [NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_NDEF_DISCOVERED)  or  [TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TECH_DISCOVERED)  intent.

Because **NFC tag deployments vary** and are many times **not under your control,** this is not always possible, **`which is why you can fallback to the other two intents when necessary.`** When you have control over the types of tags and data written, it is recommended that you use NDEF to format your tags. The following sections describe how to filter for each type of intent.


.

### Filter Each Type of Intent
#### ACTION_NDEF_DISCOVERED

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

### Create common types of NDEF records
#### createMime
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
#### createUri
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
#### createExternal
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

#### createTextRecord

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
#### createApplicationRecord

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


### ⚡Use the foreground dispatch system

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


#### enableForegroundDispatch

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

> ⚡**Note:** If you pass  `null`  for both the  `filters`  and  `techLists`  parameters that acts a wild card and will cause the foreground activity to receive all tags via the  [ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html#ACTION_TAG_DISCOVERED)  intent.

>⚡**Note:** This method must be called from the main thread, and only when the activity is in the foreground (resumed). Also, activities must call  [disableForegroundDispatch(Activity)](https://developer.android.com/reference/android/nfc/NfcAdapter.html#disableForegroundDispatch(android.app.Activity))  before the completion of their  [Activity.onPause()](https://developer.android.com/reference/android/app/Activity.html#onPause())  callback to disable foreground dispatch after it has been enabled.

>⚡**Note:** Requires the  [Manifest.permission.NFC](https://developer.android.com/reference/android/Manifest.permission.html#NFC)  permission.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNjQ0MDU2MzksLTEwMjQyNDIwNjZdfQ
==
-->



## NFC Tag:

## Tag

public final class Tag  extends  [Object](https://developer.android.com/reference/java/lang/Object.html?hl=CA) implements  [Parcelable](https://developer.android.com/reference/android/os/Parcelable.html?hl=CA) 

  
----------

Represents an NFC tag that has been discovered.

[Tag](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA)  is an immutable object that represents the **state of a NFC tag** at the time of **discovery**. It can be used as a handle to  [TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)  classes to perform advanced operations, or directly queried for its ID via  [getId()](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA#getId())  and the set of technologies it contains via  [getTechList()](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA#getTechList()). 
> Arrays passed to and returned by this class are  **_not_  cloned**, so be careful not to modify them.

A **new tag** object is created every time a **tag is discovered** (comes into range), even if it is the same physical tag. If a tag is removed and then returned into range, then only the most recent tag object can be successfully used to create a  [TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA).

### Tag Dispatch

>When a tag is **discovered**, a [Tag](https://developer.android.com/reference/android/nfc/Tag.html?hl=CA) object is created and passed to a single activity via the [NfcAdapter.EXTRA_TAG](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#EXTRA_TAG) extra in an [Intent](https://developer.android.com/reference/android/content/Intent.html?hl=CA) via [Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent)). 

A four stage dispatch is used to select the most appropriate activity to handle the tag. The Android OS executes each stage in order, and completes dispatch as soon as a single matching activity is found. If there are multiple matching activities found at any one stage then the Android activity chooser dialog is shown to allow the user to select the activity to receive the tag.

#### 1. Foreground activity dispatch

A foreground activity that has called [NfcAdapter.enableForegroundDispatch()](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#enableForegroundDispatch(android.app.Activity,%20android.app.PendingIntent,%20android.content.IntentFilter[],%20java.lang.String[][])) is given priority. See the documentation on [NfcAdapter.enableForegroundDispatch()](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#enableForegroundDispatch(android.app.Activity,%20android.app.PendingIntent,%20android.content.IntentFilter[],%20java.lang.String[][]))for its usage.

#### 2. NDEF data dispatch

If the tag contains NDEF data the system inspects the first [NdefRecord](https://developer.android.com/reference/android/nfc/NdefRecord.html?hl=CA) in the first[NdefMessage](https://developer.android.com/reference/android/nfc/NdefMessage.html?hl=CA). If the record is a URI, SmartPoster, or MIME data[Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent)) is called with [NfcAdapter.ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_NDEF_DISCOVERED). For URI and SmartPoster records the URI is put into the intent's data field. For MIME records the MIME type is put in the intent's type field. This allows activities to register to be launched only when data they know how to handle is present on a tag. This is the preferred method of handling data on a tag since NDEF data can be stored on many types of tags and doesn't depend on a specific tag technology. See [NfcAdapter.ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_NDEF_DISCOVERED) for more detail. If the tag does not contain NDEF data, or if no activity is registered for [NfcAdapter.ACTION_NDEF_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_NDEF_DISCOVERED) with a matching data URI or MIME type then dispatch moves to stage 3.

#### 3. Tag Technology dispatch

[Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent)) is called with [NfcAdapter.ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TECH_DISCOVERED) to dispatch the tag to an activity that can handle the technologies present on the tag. Technologies are defined as sub-classes of [TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA), see the package [android.nfc.tech](https://developer.android.com/reference/android/nfc/tech/package-summary.html?hl=CA). The Android OS looks for an activity that can handle one or more technologies in the tag. See [NfcAdapter.ACTION_TECH_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TECH_DISCOVERED) for more detail.

#### 4. Fall-back dispatch

If no activity has been matched then [Context.startActivity(Intent)](https://developer.android.com/reference/android/content/Context.html?hl=CA#startActivity(android.content.Intent)) is called with[NfcAdapter.ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TAG_DISCOVERED). This is intended as a fall-back mechanism. See [NfcAdapter.ACTION_TAG_DISCOVERED](https://developer.android.com/reference/android/nfc/NfcAdapter.html?hl=CA#ACTION_TAG_DISCOVERED).



.

------------------------------------
### getId

added in  [API level 10](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html?hl=CA#ApiLevels)

public byte[] getId ()

Get the Tag Identifier (if it has one).

The tag identifier is a low level serial number, used for anti-collision and identification.

Most tags have a stable unique identifier (UID), but some tags will generate a random ID every time they are discovered (RID), and there are some tags with no ID at all (the byte array will be zero-sized).

The size and format of an ID is specific to the RF technology used by the tag.

This function retrieves the ID as determined at discovery time, and does not perform any further RF communication or block.


.

------------------------------


### writeToParcel

added in  [API level 10](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html?hl=CA#ApiLevels)

public void writeToParcel ([Parcel](https://developer.android.com/reference/android/os/Parcel.html?hl=CA) dest,
                int flags)

Flatten this object in to a Parcel.

.

-------------------------------------------

### getTechList

added in  [API level 10](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html?hl=CA#ApiLevels)

public [String[]](https://developer.android.com/reference/java/lang/String.html?hl=CA) getTechList ()

Get the technologies available in this tag, as fully qualified class names.

A technology is an implementation of the  [TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)  interface, and can be instantiated by calling the static  `get(Tag)  method on the implementation with this Tag. The  [TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)  object can then be used to perform advanced, technology-specific operations on a tag.

Android defines a mandatory set of technologies that must be correctly enumerated by all Android NFC devices, and an optional set of proprietary technologies. See  [TagTechnology](https://developer.android.com/reference/android/nfc/tech/TagTechnology.html?hl=CA)  for more details.

The ordering of the returned array is undefined and should not be relied upon.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTYwMzE2MDZdfQ==
-->

## Ndef Record:

--------------------------------------------

### Create common types of NDEF records
#### createMime
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
#### createUri
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
#### createExternal
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

#### createTextRecord

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
#### createApplicationRecord

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
#### Android Application Reccords (AAR)

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


### ⚡Use the foreground dispatch system

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

	>  留意pendingIntent 中的 FLAG_ACTIVITY 要符合Manifect.xml 中的設定；假定Manifest.xml中設定了singleTask, 則pendingIntent的設定如下

	```kotlin 
	fun genPendingIntent(activity:Activity, cls: Class<*>):PendingIntent{
	   return PendingIntent.getActivity(activity.applicationContext, 0,
	     Intent(activity.applicationContext, cls)
	     .addFlags(Intent.FLAG_ACTIVITY_BROUGHT_TO_FRONT), 0)
	}
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDczNzg0MDQzLDE4OTI4OTY0NjEsNjQxOD
M4OTcsMTExNTU4NzIwNCwxMjAyNDc4NTI4LDM1MDY1MTk4N119

-->


## Trouble Shoot - A

### Error:Program type already present:
It's a dependencies conflict 相依衝突，多半為不同的相依間存在不同版本的相依

---------------------------------------------------------
### INSTALL_FAILED_VERSION_DOWNGRADE
安卓新版本的 ADT 在安裝apk的時候，會檢查當前要運行的版本 AndroidManifest.xml 中的 versionCode 和手機中已經安裝過的該程序的 versionCode 做對比，如果手機中的 versionCode 比較高，ADT 就會提示出錯：INSTALL_FAILED_VERSION_DOWNGRADE，可修改versionCode 或者uninstall apk

---------------------------------------------------------
### Failed linking file resources | failed processing manifest error
可能是AndroidManifest等XML 文件中所引用的參考不存在，如引用了@mipmap/ic_launch，但是res folder下卻沒有mipmap這個folder，或者當專案有母子project的狀態下，母子project各自指向的res位置出錯或者被子project給覆寫。如下(為子project)
```groovy
ndroid {  
	sourceSets {  
		main {  
			res.srcDirs = ['src']  
```

---------------------------------------------------------
### Cannot read packageName from path/to/AndroidManifest.xml
1. AndroidManifest.xml 放錯地方，如在sourceSet 設定了 main.manifest.srcFile 但卻與實體位置不同
	```groovy
	android {  
	sourceSets {  
		main {  
			manifest.srcFile "src/main/AndroidManifest.xml"  
	```
3. 在AndroidManifest.xml 中沒有設定package
	- 可在defaultConfig中填上applicationId
	```groovy
	defaultConfig {  
		// applicationId will replace package name specified in AndroidManifest.xml  
		applicationId "com.gknot"  
	}
	```

---------------------------------------------------------
### AndroidManifest.xml: Error: 'A' is not a valid file-based resource name character
當我們誤把 AndroidManifest.xml 放到 res 所指定的地方時，程式會把AndroidManifest.xml 視為 resource並套用resource的命名規則，如下設定
```groovy
ndroid {  
	sourceSets {  
		main {  
		manifest.srcFile "src/main/AndroidManifest.xml"  
		res.srcDirs = ['src']  
```

--------------------------------------
### strings_en.arb: Error: The file name must end with .xml
strings_en.arb - 為 Flutter i18n 插件所產生的


----------------------------------------


# Aqueduct
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Auth 2.0:](#auth-20)
- [Overview](#overview)
  - [Tasks](#tasks)
- [What is OAuth 2.0?](#what-is-oauth-20)
- [Auth Code Controller:](#auth-code-controller)
- [Issue Authorization Codes with AuthCodeController](#issue-authorization-codes-with-authcodecontroller)
- [Auth Controller:](#auth-controller)
- [Issue Access Tokens with AuthController](#issue-access-tokens-with-authcontroller)
- [CRUD:](#crud)
- [Inserting, Updating, Deleting and Fetching Objects](#inserting-updating-deleting-and-fetching-objects)
  - [Specifying Result Properties](#specifying-result-properties)
- [Security Route:](#security-route)
- [Securing Routes with Authorizer](#securing-routes-with-authorizer)
- [Setup Auth:](#setup-auth)
- [Creating AuthServers to Authenticate and Authorize](#creating-authservers-to-authenticate-and-authorize)
  - [Creating Instances of AuthServer and AuthServerDelegate](#creating-instances-of-authserver-and-authserverdelegate)
  - [Using ManagedAuthDelegate](#using-managedauthdelegate)
- [Understand Authentication:](#understand-authentication)
  - [Understanding Login Authentication](#understanding-login-authentication)
- [Codecs:](#codecs)
  - [Streaming Response Bodies](#streaming-response-bodies)
- [Codecs and Content Types](#codecs-and-content-types)
  - [register codecs into CodeRegistery](#register-codecs-into-coderegistery)
  - [Compression with gzip](#compression-with-gzip)
- [Serializable Objects](#serializable-objects)
  - [Serializable and OpenAPI Generation](#serializable-and-openapi-generation)
- [🔥 Example: multipart/form-data](#-example-multipartform-data)
- [Processing MIME multipart media types  #](#processing-mime-multipart-media-types--)
- [Core Concept:](#core-concept)
- [Core Concepts](#core-concepts)
  - [Resources](#resources)
  - [Routing](#routing)
  - [Controllers](#controllers)
  - [Request Binding @bind](#request-binding-bind)
  - [Using ManagedContext to Connect to a Database](#using-managedcontext-to-connect-to-a-database)
  - [Configuration Files](#configuration-files)
- [DataBase:](#database)
- [Example: One-to-Many Relationship](#example-one-to-many-relationship)
  - [Example: Hierarchical Relationships (Self Referencing)](#example-hierarchical-relationships-self-referencing)
- [Example: Many-to-Many Relationship](#example-many-to-many-relationship)
- [ManagedObject Serialization and Deserialization](#managedobject-serialization-and-deserialization)
- [Basic Behavior](#basic-behavior)
- [Behavior of Null Values](#behavior-of-null-values)
- [Behavior of Transient Properties](#behavior-of-transient-properties)
- [Auth](#auth)
- [JSON:](#json)
- [JSON Document Storage](#json-document-storage)
  - [JSON Columns in Relational Databases](#json-columns-in-relational-databases)
  - [The Document Data Type](#the-document-data-type)
  - [Basic Operations on Document Properties](#basic-operations-on-document-properties)
  - [Fetching Sub-documents](#fetching-sub-documents)
- [Multipart:](#multipart)
  - [Multipart header](#multipart-header)
- [Resource Controller:](#resource-controller)
- [ResourceController](#resourcecontroller)
  - [Operation Methods](#operation-methods)
  - [Routing to a ResourceController](#routing-to-a-resourcecontroller)
  - [Request Bindings](#request-bindings)
  - [Other ResourceController Behavior](#other-resourcecontroller-behavior)
- [Serving Files:](#serving-files)
- [Serving Files and Caching](#serving-files-and-caching)
  - [FileController](#filecontroller)
  - [Caching](#caching)
  - [File Serving and Caching Outside of FileController](#file-serving-and-caching-outside-of-filecontroller)
- [Validation:](#validation)
- [Validating Data](#validating-data)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## Auth 2.0:
## Overview

### Tasks

Aqueduct has types to manage authentication and authorization according to the  [OAuth 2.0 specification](https://tools.ietf.org/html/rfc6749).

You create an  **`AuthServer`  service object** for your application that **manages authentication** and **authorization logic**. 

An  `AuthServer`  requires a helper object that implements  **`AuthServerDelegate`**  to handle configuration and required data storage. Most often, this object is a  **`ManagedAuthDelegate<T>`**  that uses the Aqueduct ORM to manage this storage.

An  **`AuthServer`  service object** is injected into  **`Authorizer`  controllers** that protect access to controller channels. An  `AuthServer`  is also injected into  **`AuthCodeController`  and  `AuthController`**  to provide HTTP APIs for authentication.

The  `aqueduct auth`  command-line tool manages configuration - such as client identifier management - for live applications.

![Authorization Objects](https://aqueduct.io/docs/img/authobjects.png)



## What is OAuth 2.0?

Most applications have the concept of a user. To prevent just anyone from saying they are this user, the user has a password. When a user wants to use your application, they send their username and password to the server so it can ensure they are who they say they are.

The simple way to do this is to send the username and password in an Authorization header for every request. Two reasons this is bad: - 

- first, every time the user wants to do something, their password is sent in every request and that can be unsafe. 
- Second, any application that wants to keep the user logged in has to store that password - and that is unsafe.

In OAuth 2.0, the user gives their username and password to a client application **once**. 

- The application sends those credentials to a server
- The server checks the token, makes sure its not expired, and then lets the application's request go through. 
- The application doesn't have to store the password and doesn't have to ask the user for their password again.
- the server gives the application back an access token. 

> 📝 A access token is a long, random string that no one can guess. When the user wants to do more stuff, the application sends the **token** with **every request**, instead of the user's **password**. 

This **credential-for-token exchange** happens by sending a **POST** request to some endpoint, where the username and password are sent in the request body. Typically, an Aqueduct application's route for this is  `/auth/token`  and handled by an instance of  [AuthController](https://aqueduct.io/docs/auth/controllers/).

OAuth 2.0 makes a subtle distinction: a user and the application they are using are not the same thing. It's intuitive to think of a user as "making a request to the server", but in reality, the user makes a request to an application and  _the application_  makes the request to the server. The server  _grants_  the application access on behalf of the user. In other words, when a user enters their credentials into an application, the application goes to the server and says "Hey, this user said I can do stuff for them. Look, this is their secret password!"

This is an important distinction, because an **OAuth 2.0 server doesn't just verify users: it also verifies applications.** An application has a identifier that has been registered with the server. 

So, for an ecosystem that has a web client, an Android and an iOS app there would likely be three identifiers - one for each. 
> 📝 NOTE:
> - user identifier
> - app identifier
> - platform identifier

The client application usually stores that identifier in a database. This identifier is called a  **_client identifier_**. Client identifiers are added to Aqueduct applications with the  `aqueduct auth`  tool (see  [Aqueduct Auth CLI](https://aqueduct.io/docs/auth/cli/)).

When the user is logging in through an application, they submit their username and password. The user doesn't provide the client identifier - in fact, the user doesn't know it. 

The application sends a request with the user's username and password in the request body, and the client identifier in the Authorization header. All three have to check out for the server to give the application back a token. The full request in pseudo-code looks something like:


```dart
var request = HTTPRequest("/auth/token");
request.method = "POST";
request.contentType = "application/x-www-form-urlencoded";
request.authorization = Base64.encode("$clientID:");
request.body = {
  "username" : "bob@stablekernel.com",
  "password" : "supersecretstuff",
  "grant_type" : "password"
};
```

An access token can expire. How long it takes to expire is up to the server - Aqueduct defaults to 24 hours. At first glance, this means that the application would have to ask the user for a password again. But, tokens can also be refreshed. **Refreshing a token grants a brand new access token**, but **without** having to ask for the password. This is possible because an access token comes with a  **_refresh token_**. The refresh token is another long, random string. So, the JSON the server sends back when granting a token looks like this:

**The access token**
```json
{
  "access_token" : "Abca09zzzza2o2kelmzlli3ijlka",
  "token_type" : "bearer",
  "refresh_token" : "lkmLIAmooa898nm20jannnnnxaww",
  "expire_in" : 3600
}
```

The application hangs on to both an access token and a refresh token. **When the token expires, it will send the refresh token back to the server** to get a replacement access token. This is done through the same route that the access token came from -  `/auth/token`  - except the parameters are a bit different:

**The refresh token**
```dart
var request = HTTPRequest("/auth/token");
request.method = "POST";
request.contentType = "application/x-www-form-urlencoded";
request.authorization = Base64.encode("$clientID:");
request.body = {
  "refresh_token" : "lkmLIAmooa898nm20jannnnnxaww",
  "grant_type" : "refresh_token"
};
```

Exchanging a refresh token has the same response as the initial exchange for username and password - except a few values will have changed.

The verification and storage of authorization and authentication information is managed by an  [AuthServer](https://aqueduct.io/docs/auth/server/).



#### Other Methods for Obtaining Authorization

The method of getting a token above - sending a username and password to  `/auth/token`  - is just one of four possible methods OAuth 2.0 uses to authenticate a user. This particular one is called the  _resource owner password credentials grant_. A resource owner is a fancy word for a 'user'. We can shorten it up to just the **'password flow'**. It's probably the most common flow - mobiles applications and front-end web applications often use this flow. When you enter your credentials, the client application sends them directly to the server.

The other commonly used flow **prevents** the client application from ever **seeing the user's credentials**. For example, you might sign into Pivotal Tracker with your Google account. Your account on Pivotal Tracker **doesn't have** a password. Instead, it is **linked to** your Google account - which does. Pivotal Tracker never sees your Google password. When you login to Pivotal Tracker in this way, it takes you to Google's authentication page - owned and operated by Google. When you login successfully, Google gives Pivotal Tracker your token. Pivotal Tracker is now an application that can do things on your behalf.

This is called the  _authorization code grant_  - or just 'auth code flow'. An instance of  `AuthCodeController`  handles granting authorization codes. Once a code is received, it can be exchanged for a token via an  `AuthController`.











<!--stackedit_data:
eyJoaXN0b3J5IjpbNDAyMzgyMzE4LDIwNzIzMzA3OTgsNTMyMT
IwNTg2LDMxNzMyNjI0MiwxNzE3MjE5MDksLTYyODQwMTgwNyw0
MzYxMTI0NzBdfQ==
-->

## Auth Code Controller:

## Issue Authorization Codes with AuthCodeController

An  `AuthCodeController`  manages the OAuth 2.0 authorization code flow. The authorization code flow is used when an Aqueduct application **allows third party** applications access to authorized resources.

Let's say you've built an Aqueduct application that allows people to store notes for themselves. Now, a friend approaches you with their application that is a to-do list. Instead of building their own note-taking feature, your friend wants users of their application to access the notes the user has stored in your application. While trustworthy, you don't want your friend to have access to the username and passwords of your subscribers.

Your friend adds a link to their application that takes the user to an HTML page hosted by your server. The user enters their credentials in this page, which sends a  `POST`  request to your server. Your server responds by redirecting the user's browser back into your friend's application. An  _authorization code_  is included in the query string of the redirect URL.

Your friend's application parses the code from the URL and sends it to  _their_  server. Behind the scenes, their server exchanges this code with your server for an access token.

An  `AuthCodeController`  responds to both  `GET`  and  `POST`  requests. When issued a  `GET`, it serves up a HTML page with a login form. This login form's submit action sends a  `POST`  to the same endpoint with the username and password of the user. Upon success, the response from the  `POST`  is a 302 redirect with an authorization code.

Setting up an  `AuthCodeController`  is nearly as simple as setting up an  `AuthController`, but requires a function that renders the HTML login form. Here's an example:

```dart
@override
Controller get entryPoint {
  final router = Router();
  router
    .route("/auth/code")
    .link(() => AuthCodeController(
      authServer, renderAuthorizationPageHTML: renderLogin));

  return router;
}

Future<String> renderLogin(
    AuthCodeController requestingController,
    URI requestURI,
    Map<String, String> queryParameters) {
  var html = HTMLRenderer.templateWithSubstitutions(
    "web/login.html", requestURI, queryParameters);

  return html;
}
```

It is important that all values passed to HTML rendering function are sent in the form's query parameters - they contain necessary security components and scope information.

When your friend's application links to your login page -  `GET /auth/code`  - they must include three query parameters:  `state`,  `client_id`,  `response_type`. They may optionally include  `scope`.

https://stablekernel.com/auth/code?client_id=friend.app&response_type=code&state=87uijn3rkja

The value of  `client_id`  must be created specifically for your friend's application and stored in your database. (See more on generating client identifiers with  `aqueduct auth`  in  [Aqueduct Auth CLI](https://aqueduct.io/docs/auth/cli/).) The  `response_type`  must always be  `code`. The  `state`  must be a value your friend's application creates - it is often some random value like a session cookie.

When a user of your friend's application goes through this process, they are redirected back into your friend's application. Both the generated authorization code and the value for  `state`  will be query parameters in the URL. That redirect URL will look like:

https://friends.app/code_callback?code=abcd672kk&state=87uijn3rkja

The redirect URL is pre-determined when generating the client identifier with  `aqueduct auth`.

Your friend's application verifies that  `state`  matches the  `state`  they sent in  `GET /auth/code`. They then send the  `code`  to their server. The server then exchanges this code with your server by issuing a  `POST`  to an  `AuthController`  -  _NOT_  the  `AuthCodeController`  - with the following  `application/x-www-form-urlencoded`  body:

grant_type=authorization_code&code=abcd672kk

An access token will be returned to the server which your friend then stores in their database. Whenever one of their users makes a request that requires accessing your application's data, they will execute requests with that access token.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MTA1NzM4MTYsLTIxNDU3MjkzMDRdfQ
==
-->

## Auth Controller:
## Issue Access Tokens with AuthController

An application using Aqueduct's Auth framework must have endpoints to exchange credentials for access tokens. While a developer could implement these endpoints themselves and talk directly to an  `AuthServer`, the OAuth 2.0 specification is where happiness goes to die. Therefore, there exist two  `Controller`s in Aqueduct that handle granting and refreshing authorization tokens -  `AuthController`  and  `AuthCodeController`.

#### Issue, Refresh and Exchange Tokens with AuthController

An  `AuthController`  
- grants access tokens 
- refreshes tokens 
- exchanges authorization codes obtained from  `AuthCodeController`  for access tokens.

Using an  `AuthController`  in an application is straightforward - hook it up to a  `Router`  and pass it an  `AuthServer`.

```dart
@override
Controller get entryPoint {
  final router = Router();
  router
    .route("/auth/token")
    .link(() => AuthController(authServer));
  return router;
}
```

To grant an **access token**, a client application sends a HTTP  **`POST`**  to the controller. The request must have:

-   an Authorization header with the **Client ID** and **Client Secret** (if one exists) and,
-   a  **`x-www-form-urlencoded`**  body with the username and password of the authenticating user.

The body must also contain the key-value pair  **`grant_type=password`**. For example, the following Dart code will initiate successful authentication:

##### client request:
```dart
var clientID     = "com.app.demo";
var clientSecret = "mySecret";
var body = "username=bob@stablekernel.com&password=foobar&grant_type=password";
var clientCredentials = Base64Encoder().convert("$clientID:$clientSecret".codeUnits);
var response          = await http.post(
  "https://stablekernel.com/auth/token",
  headers: {
    "Content-Type": "application/x-www-form-urlencoded",
    "Authorization": "Basic $clientCredentials"
  },  body: body);
```

If the OAuth 2.0 client ID is public - that is, it does not have a client secret - the secret is omitted from the authorization header:

```dart
// Notice that the separating colon (:) is still present.
var clientCredentials = Base64Encoder().convert("$clientID:".codeUnits);
```

The response to a password token request is a JSON body that follows the OAuth 2.0 specification:
##### server response
```dart
{
  "access_token": "..."
  "refresh_token": "...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

The  `expires_in`  field is a computed property based on the delta of the issue date and expiration date. You should avoid manually editing the values for the columns  `issuedate`  and  `expirationdate`

> Tokens are refreshed through **the same** endpoint, but with a payload that only contains the **refresh token** and  **`grant_type=refresh_token`**.

```dart
var body = "username=bob@stablekernel.com&password=foobar"
"&grant_type=refresh_token&refresh_token=kjasdiuz9u3namnsd";
```

See  [Aqueduct Auth CLI](https://aqueduct.io/docs/auth/cli/)  for more details on creating OAuth 2.0 client identifier and secrets.

If an Aqueduct application is using scope, an additional  `scope`  parameter can contain a space-delimited list of requested authorization scope. Only allowed scopes are returned and granted, and if no scopes are allowed then the request fails. If scope is provided, granted scope will be available in the response body.

It is important that an  `Authorizer`  _must not_  protect instances of  `AuthController`. The Authorization header is parsed and verified by  `AuthController`.

Once granted, an access token can be used to pass  `Authorizer.bearer()`s in the application channel.



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzODY4NjY4NiwzNzQxNjkyMzQsMTc5NT
c5NTMxNSwtMTczNDg1OTM0MSwxMDIyMzY5NDc0LDE3MzM1NTA5
OTNdfQ==
-->



## CRUD:

## Inserting, Updating, Deleting and Fetching Objects

To send commands to a database - whether to fetch, insert, delete or update objects - you will create, configure and execute instances of  `Query<T>`. The type argument must be a subclass of  `ManagedObject`, which determines the table the query will operate on.

A query compiles and executes a SQL query and requires a  [ManagedContext](https://aqueduct.io/docs/db/connecting.db)  to determine the database to connect to. Here's an example of a  `Query<T>`  that fetches all instances of  `User`:

final query = Query<User>(context);
final allUsers = await query.fetch();

A  `Query<T>`  has four basic execution methods:  `fetch`,  `update`,  `insert`,  `delete`.

-   `fetch`  will retrieve data from a database (it is equivalent to the SQL operation  `SELECT`).
-   `update`  will modify existing data in a database (it is equivalent to the SQL operation  `UPDATE`).
-   `insert`  will add new data to a database (it is equivalent to the SQL operation  `INSERT`).
-   `delete`  will remove data from a database (it is equivalent to the SQL operation  `DELETE`).

A  `Query<T>`  has many configurable properties. These properties will impact which objects get fetched, the data that gets sent to the database, the order that data is returned in, and so on.

In the following sections, assume that a  `User`  managed object subclass exists that is declared like so:

class User extends ManagedObject<_User> implements _User {}
class _User {
  @primaryKey
  int id;

  @Column(indexed: true)
  String email;

  String name;
}

#### Inserting Data with a Query

To insert data with a query, you create a new  `Query<T>`  object, configure its  `values`  property and then call its  `insert()`  method.

final query = Query<User>(context)
  ..values.name = "Bob"
  ..values.email = "bob@stablekernel.com";  

final user = await query.insert();  

The  `values`  of a  `Query<T>`  is an instance of  `T`  (the managed object type you are inserting). You can configure individual properties of  `values`, or you can assign  `values`  to an instance you have created elsewhere:

final userValues = User()
  ..name = "Bob"
  ..email = "bob@stablekernel.com";
final query = Query<User>(context)..values = userValues;
final user = await query.insert();  

Either way, this query is translated into the following SQL:

INSERT INTO _user (name, email) VALUES ('Bob', 'bob@stablekernel.com') RETURNING id, name, email;

Notice that only the values set on the  `values`  object are included in the SQL INSERT query. In this example, both  `name`  and  `email`  were set, but not  `id`  and therefore only  `name`  and  `email`  were included in the query. (In this case, the primary key is auto-incrementing and the database will generate it.)

Values that are explicitly set to  `null`  will be sent as  `NULL`. For example, consider the following  `Query<T>`  and its SQL:

var query = Query<User>(context)
  ..values.name = null
  ..email = "bob@stablekernel.com";

// INSERT INTO _user (name, email) VALUES (NULL, 'bob@stablekernel.com') RETURNING id, name, email;
await query.insert();

An insert query will return a managed object that represents the row that is inserted.

Prefer the Inserted Object

After you insert an object, you should prefer to use the object returned by an insert query rather than the values you used to populate the query. The object returned from the query will be an accurate representation of the database row, while the object used to build the query may be incomplete or different. For example, an auto-incrementing primary key won't be available in your query-building instance, but will in the object returned from the successful query.

There is one difference to note when choosing between assigning an instance to  `values`, or configuring the properties of  `values`. In an instance you create, a relationship property must be instantiated before accessing its properties. When accessing the relationship properties of  `values`, an empty instance of the related object is created immediately upon access.

final employee = Employee()
  ..manager.id = 1; // this is a null pointer exception because manager is null

final query = Query<Employee>(context)
  ..values.manager.id = 1; // this is OK because of special behavior of Query

Once you assign an object to  `values`, it will adopt the behavior of  `values`  and instantiate relationships when accessed. Also note that after assigning an object to  `values`, changes to the original object are not reflected in  `values`. In other words, the object is copied instead of referenced.

For simple insertions and for inserting more than one object, you can use the methods on the context:

final context = ManagedContext(...);
final bob = User()..name = "Bob";
final jay = User()..name = "Jay";

final insertedObject = await context.insertObject(bob);
final insertedObjects = await context.insertObjects([bob, jay]);

#### Updating Data with a Query

Updating rows with a  `Query<T>`  is similar to inserting data: you set the  `Query.values`  for properties you want to change. The type parameter for the  `Query<T>`  indicates which database table will get updated when the query is executed.

An update query can - and likely should - be restricted to a single row or subset of rows. This is done by configuring the  `Query.where`  property - which gets translated into the  _where clause_  of the SQL command. Here's an example:

// A Query that will change any user's whose name is 'Bob' to 'Fred'
var query = Query<User>(context)
  ..values.name = "Fred"
  ..where((u) => u.name).equalTo("Bob");

List<User> bobsThatAreNowFreds = await query.update();

Like  `values`,  `where`  is also the same managed object type the query is being executed on. In the above example, then, both  `values`  and  `where`  and instances of  `User`. This query executes the following SQL:

UPDATE _user SET name='Fred' WHERE name='Bob';

The  `where`  property is a very powerful and flexible, and so there is an entire section dedicated to it later in this guide. For now, we'll stick to some of the things specific to  `update()`.

Like  `insert()`, only the values set in the  `values`  property of a query get updated when executing  `update()`. Values that are omitted are not included. Values that need to be set to  `null`  must explicitly be set to  `null`  in the query:

// A Query that will remove names from anyone currently named Bob.
var query = Query<User>(context)
  ..values.name = null
  ..where((u) => u.name).equalTo("Bob");

An update query returns every modified row as a result. If no rows are updated, the return value is an empty list.

There is a variant to  `Query<T>.update`  named  `updateOne`. The  `updateOne`  method will build and execute a SQL query in the same way a normal  `update`  does, however, it will only return the single instance that was updated instead of a list. This is convenience method for the caller to get back a single instance instead of a list:

// Update user with id = 1 to have the name 'Fred'
var query = Query<User>(context)
  ..values.name = "Fred"
  ..where((u) => u.id).equalTo(1);

var updatedUser = await query.updateOne();

The  `updateOne`  method will return  `null`  if no rows were updated. It is important to note that if  `updateOne`  is used and more than one row is updated,  `updateOne`  will throw an exception and the changes to the data  _are not reversible_. Because this is likely a mistake, this is considered an error, hence the exception is thrown. It is up to the programmer to recognize whether or not a particular  `updateOne`  query would impact multiple rows.

Update queries have a safety feature that prevents you from accidentally updating every row. If you try to execute a  `Query<T>`  to do an update without configuring  `where`, an exception is thrown prior to carrying out the request. If you actually want to update every row of a table, you must set the  `Query.canModifyAllInstances`  to  `true`prior to execution. (This property defaults to  `false`.)

#### Deleting Data with a Query

A  `Query<T>`  will delete rows from a database when using  `delete()`. Like update queries, you should specify a row or rows using  `where`  properties of the  `Query<T>`. The result of a delete operation will be a  `Future<int>`  with the number of rows deleted.

var query = Query<User>(context)
  ..where((u) => u.id).equalTo(1);

int usersDeleted = await query.delete();

Also like update queries, delete queries have a safety feature that prevents you from accidentally deleting every row in a table with  `canModifyAllInstances`.

Any properties set in the query's  `values`  are ignored when executing a delete.

#### Fetching Data with a Query

Of the four basic operations of a  `Query<T>`, fetching data is the most configurable. A simple  `Query<T>`  that would fetch every instance of some entity looks like this:

var query = Query<User>(context);

List<User> allUsers = await query.fetch();

Fetch queries can be limited to a number of instances with the  `fetchLimit`  property. You may also set the  `offset`of a  `Query<T>`  to skip the first  `offset`  number of rows. Between  `fetchLimit`  and  `offset`, you can implement naive paging. However, this type of paging suffers from a number of problems and so there is another paging mechanism covered in later sections.

A fetch  `Query<T>`  uses its  `where`  property to filter the result set, just like delete and update queries. The  `values`  of a query are ignored when fetching objects. You may also fetch a single instance with  `fetchOne`. If no instance is found,  `null`  is returned. Only use this method when the search criteria is guaranteed to be unique.

var query = Query<User>(context)
  ..where((u) => u.id).equalTo(1);

User oneUser = await query.fetchOne();

When you are fetching an object by its primary key, you can use a shorthand method  `ManagedContext.fetchObjectWithID`. The method must be able to infer the type of the object, or you must provide it:

final object = await context.fetchObjectWithID<User>(1);

#### Sorting

Results of a fetch can be sorted using the  `sortBy`  method of a  `Query<T>`. Here's an example:

var q = Query<User>(context)
  ..sortBy((u) => u.dateCreated, QuerySortOrder.ascending);

`sortBy`  takes two arguments: a closure that returns which property to sort by and the order of the sort.

A  `Query<T>`  results can be sorted by multiple properties. When multiple  `sortBy`s are invoked on a  `Query<T>`, later  `sortBy`s are used to break ties in previous  `sortBy`s. For example, the following query will sort by last name, then by first name:

var q = Query<User>(context)
  ..sortBy((u) => u.lastName, QuerySortOrder.ascending)
  ..sortBy((u) => u.firstName, QuerySortOrder.ascending);

Thus, the following three names would be ordered like so: 'Sally Smith', 'John Wu', 'Sally Wu'.

#### Property Selectors

In the section on sorting, you saw the use of a  _property selector_  to select the property of the user to sort by. This syntax is used for many other query manipulations, like filtering and joining. A property selector is a closure that gives you an object of the type you are querying and must return a property of that object. The selector  `(u) => u.lastName`  in the previous section is a property selector that selects the last name of a user.

The Dart analyzer will infer that the argument of a property selector, and it is always the same type as the object being queried. This enables IDE auto-completion, static error checking, and other tools like project-wide renaming.

Live Templates

To speed up query building, create a Live Template in IntelliJ that generates a property selector when typing 'ps'. The source of the template is  `(o) => o.$END$`. A downloadable settings configuration for IntelliJ exists  [here](https://aqueduct.io/docs/intellij/)  that includes this shortcut.

### Specifying Result Properties

When executing queries that return managed objects (i.e.,  `insert()`,  `update()`  and  `fetch()`), the default properties for each object are fetched. The default properties of a managed object are properties that correspond to a database column - attributes declared in the table definition. A managed object's default properties can be modified when declaring its table definition:

class _User {
  @Column(omitByDefault: true)
  String hashedPassword;
}

Any property with  `omitByDefault`  set to true will not be fetched by default.

A property that is  `omitByDefault`  can still be fetched. Likewise, a property that is in the defaults can still be omitted. Each  `Query<T>`  has a  `returningProperties`  method to adjust which properties do get returned from the query. Its usage looks like this:

var query = Query<User>(context)
  ..returningProperties((user) => [user.id, user.name]);

`returningProperties`  is a multiple property selector - instead of returning just one property, it returns a list of properties.

You may include 'belongs-to' relationships in  `returningProperties`, but you may not include 'has-many' or 'has-one' relationships. An exception will be thrown if you attempt to. To include properties from relationships like these, see  [join in Advanced Queries](https://aqueduct.io/docs/db/advanced_queries/).

Note that if you omit the primary key of a managed object from  `returningProperties`, it will automatically be added. The primary key is necessary to transform the rows into instances of their  `ManagedObject<T>`  subclass.

#### Exceptions and Errors

When executing a query, it may fail for any number of reasons: the query is invalid, a database couldn't be reached, constraints were violated, etc. In many cases, this exception originates from the underlying database driver. When thrown in a controller, these exceptions will trigger a 500 Server Error response.

Exceptions that are thrown in response to user input (e.g., violating a database constraint, invalid data type) are re-interpreted into a  `QueryException`  or  `ValidationException`. Both of these exception types have an associated  `Response`  object that is sent instead of the default 500 Server error.

For this reason, you don't need to catch database query exceptions in a controller; an appropriate response will be sent on your behalf.

#### Statement Reuse

Aqueduct will parameterize and reuse queries when possible. This allows for significant speed and security improvements. Note that you do not have to do anything special to take advantage of this feature. However, currently at this time, you may not disable this feature.






<!--stackedit_data:
eyJoaXN0b3J5IjpbOTEyNzIxNTQ3XX0=
-->



## Security Route:


## Securing Routes with Authorizer

Instances of  `Authorizer`  are added to an application channel to verify HTTP request's authorization information before passing the request onwards. They protect channel access and typically come right after  `route`. Here's an example:

```dart
@override
Controller get entryPoint {
  final router = Router();

  router
    .route("/protected")
    .link(() => Authorizer.bearer(authServer))
    .link(() => ProtectedController());

  router
    .route("/other")
    .link(() => Authorizer.basic(authServer))
    .link(() => OtherProtectedController());

  return router;
}
```

An  `Authorizer`  parses the Authorization header of an HTTP request. The named constructors of  `Authorizer`indicate the required format of Authorization header. 

- The  **`Authorization.bearer()`** 
   constructor expects an OAuth 2.0 bearer token in the header, which has the following format:

	- Authorization: **Bearer 768iuzjkx82jkasjkd9z9**

>  ##### 📝 **bear** - a word borrowed from client-certificate authenticcateion
>  Client-certificate authentication is a more secure method of authentication than either basic or form-based authentication. It **uses HTTP over SSL**, in which the server and, optionally, the client **authenticate one another** using public key certificates.  Secure Socket Layer  (SSL) provides data encryption, server authentication, message integrity, and optional client authentication for a TCP/IP connection. **You can think of a  public key certificate  as the digital equivalent of a passport**. It is issued by a trusted organization, which is called a  certificate authority  (CA), and provides identification for the **bearer**.

- **`Authorizer.basic`**  
  expects HTTP Basic Authentication, where the username and password are joined with the colon character (`:`) and Base 64-encoded:

	- // 'dXNlcjpwYXNzd29yZA==' is 'user:password'
	Authorization: **Basic dXNlcjpwYXNzd29yZA==**


If the header can't be parsed, doesn't exist or is in the wrong format, an  `Authorizer`  responds to the request with a 401 status code and prevents the next controller from receiving the request.

> ⚡ Once parsed, an  `Authorizer`  sends the information - either the bearer token, or the username and password - to its  `AuthServer`  for verification. If the  `AuthServer`  rejects the authorization info, the  `Authorizer`  responds to the request with a 401 status code and prevents the next controller from receiving the request. Otherwise, the request continues to the next controller.

- For  `Authorizer.bearer`
the value in a request's header must be a valid, **unexpired access token**. These types of authorizers are used when an endpoint requires a **logged in user**.

- For  `Authorizer.basic`  
authorizers, credentials are verified by finding an OAuth 2.0 client identifier and ensuring its client secret matches. Routes with this type of authorizer are known as  **_client authenticated_**  routes. These types of authorizers are used when an endpoint requires a **valid client application**, but **not** a logged in user.

#### Authorizer and OAuth 2.0 Scope

An  `Authorizer`  may restrict access to controllers based on the scope of the request's bearer token. By default, an  `Authorizer.bearer`  allows any valid bearer token to pass through it. If desired, an  `Authorizer`  is initialized with a list of required scopes. A request may only pass the  `Authorizer`  if it has access to  _all_  scopes listed in the  `Authorizer`. For example, the following requires at least  `user:posts`  and  `location`  scope:

```dart
router
  .route("/checkin")
  .link(() => Authorizer.bearer(authServer, scopes: ["user:posts", "location"]))
  .link(() => CheckInController());
```

Note that you don't have to use an  `Authorizer`  to restrict access based on scope. A controller has access to scope information after the request has passed through an  `Authorizer`, so it can use the scope to make more granular authorization decisions.

#### Authorization Objects

For example, a social networking application might have a  `/news_feed`  endpoint protected by an  `Authorizer`. When an authenticated user makes a request for  `/news_feed`, the controller will return that user's news feed. It can determine this by using the  `Authorization`:

```dart
class NewsFeedController extends ResourceController {
  NewsFeedController(this.context);
  ManagedContext context;

  @Operation.get()
  Future<Response> getNewsFeed() async {
    var forUserID = request.authorization.ownerID;

    var query = Query<Post>(context)
      ..where((p) => p.author).identifiedBy(forUserID);

    return Response.ok(await query.fetch());
  }
}
```
In the above controller, it's impossible for a user to access another user's posts.

`Authorization`  objects also retain the scope of an access token so that a controller can make more granular decisions about the information/action in the endpoint. Checking whether an  `Authorization`  has access to a particular scope is accomplished by either looking at the list of its  `scopes`  or using  `authorizedForScope`:

```dart
class NewsFeedController extends ResourceController {
  NewsFeedController(this.context);
  ManagedContext context;

  @Operation.get()
  Future<Response> getNewsFeed() async {
    if (!request.authorization.authorizedForScope("user:feed")) {
      return Response.unauthorized();
    }
    var forUserID = request.authorization.ownerID;
    var query = Query<Post>(context)
      ..where((p) => p.author).identifiedBy(forUserID);

    return Response.ok(await query.fetch());
  }
}
```

#### Using Authorizers Without AuthServer

Throughout this guide, the argument to an instance of  `Authorizer`  has been referred to as an  `AuthServer`. This is true - but only because  `AuthServer`  implements  `AuthValidator`.  `AuthValidator`  is an interface for verifying bearer tokens and username/password credentials.

You may use  `Authorizer`  without using  `AuthServer`. For example, an application that **doesn't** use OAuth 2.0 could provide its own  `AuthValidator`  interface to simply verify the username and password of every request:

```dart
class BasicValidator implements AuthValidator {
  @override
  FutureOr<Authorization> validate<T>(AuthorizationParser<T> parser, T authorizationData, {List<AuthScope> requiredScope}) {}
    var user = await userForName(usernameAndPassword.username);
    if (user.password == hash(usernameAndPassword.password, user.salt)) {
      return Authorization(...);
    }

    // Will end up creating a 401 Not Authorized Response
    return null;
  }
}
```

- The  `validate`  method must return an  
	- **`Authorization`**  if the credentials are **valid**, or 
	- **null** if they are **not**. 

The  `parser`  lets the validator know the format of the Authorization header (e.g., 'Basic' or 'Bearer') and  `authorizationData`  is the meaningful information in that header. 

There are two concrete types of  `AuthorizationParser<T>`
- `AuthorizationBasicParser` : 
	The authorization data for a basic parser is an instance of  `AuthBasicCredentials`  that contain the username and password 
- `AuthorizationBearerParser`: 
	the bearer parser's authorization data is the bearer token string.



























<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEzODExODg1NF19
-->

## Setup Auth:
## Creating AuthServers to Authenticate and Authorize

> ⚡ An  `AuthServer`  is a service that handles 
> - creating 
> - verifying and 
> - refreshing 
> authorization tokens. You create an  `AuthServer`  in your application channel and inject into types that deal with authorization. 

This types include:

-   `Authorizer`: middleware controller that protects endpoint controllers from unauthorized access
-   `AuthController`: endpoint controller that grants **access tokens**
-   `AuthCodeController`: endpoint controller that grants authorization codes to be **exchanged** for access tokens

An  `AuthServer`  must persist the data it uses and creates - like client identifiers and access tokens. Storage is often performed by a database, but it can be in memory, a cache or some other medium. Because of the many different storage mediums, an  `AuthServer`  doesn't perform any storage itself - it relies on an instance of  `AuthServerDelegate`  specific to your application. This allows storage to be independent of verification logic.



### Creating Instances of AuthServer and AuthServerDelegate

`AuthServerDelegate`  is an interface that an  `AuthServer`  uses to handle storage of client identifiers, tokens and other authorization artifacts. An  `AuthServer`  must be created with a concrete implementation of  `AuthServerDelegate`. Aqueduct contains a concrete implementation of  `AuthServerDelegate`  that uses the ORM. It is highly recommended to use this implementation instead of implementing your own storage because it has been thoroughly tested and handles cleaning up expired data correctly.

This concrete implementation is named  `ManagedAuthDelegate<T>`. It exists in a sub-package of Aqueduct and must be explicitly imported. Here's an example of creating an  `AuthServer`  and  `ManagedAuthDelegate<T>`:

```dart
import 'package:aqueduct/aqueduct.dart';
import 'package:aqueduct/managed_auth.dart';

class MyApplicationChannel extends ApplicationChannel {  
  AuthServer authServer;
  @override
  Future prepare() async {
    final context = ManagedContext(...);
    final delegate = ManagedAuthDelegate<User>(context);
    authServer = AuthServer(delegate);
  }
  ...
}
```
While `AuthServer` has methods for handling authorization tasks, it is rarely used directly. Instead, `AuthCodeController` and `AuthController` are hooked up to routes to grant authorization tokens through your application's HTTP API. Instances of `Authorizer` secure routes in channels. All of these types invoke the appropriate methods on the `AuthServer`. Here's an example `ApplicationChannel` subclass that sets up and uses authorization:

```dart
import 'package:aqueduct/aqueduct.dart';
import 'package:aqueduct/managed_auth.dart';

class MyApplicationChannel extends ApplicationChannel {
  AuthServer authServer;
  ManagedContext context;
  @override
  Future prepare() async {
    context = ManagedContext(...);
    final delegate = ManagedAuthDelegate<User>(context);
    authServer = AuthServer(delegate);
  }
  @override
  Controller get entryPoint {
    final router = Router();
    // Set up auth token route- this grants and refresh tokens
    router.route("/auth/token").link(() => AuthController(authServer));
    // Set up auth code route- this grants temporary access codes that can be exchanged for token
    router.route("/auth/code").link(() => AuthCodeController(authServer));
    // Set up protected route
    router
      .route("/protected")
      .link(() => Authorizer.bearer(authServer))
      .link(() => ProtectedController());
    return router;
  }
}
```
### Using ManagedAuthDelegate

`ManagedAuthDelegate<T>`  is a concrete implementation of  `AuthServerDelegate`, providing storage of authorization tokens and clients for an  `AuthServer`. Storage is accomplished by Aqueduct's ORM.  `ManagedAuthDelegate<T>`, by default, is not part of the standard  `aqueduct`  library. To use this class, an application must import  `package:aqueduct/managed_auth.dart`.

The type argument to  `ManagedAuthDelegate<T>`  represents the application's concept of a 'user' or 'account' - OAuth 2.0 terminology would refer to this type as a  **_resource owner_**. 

A resource owner must be a  `ManagedObject<T>`  subclass that is specific to your application. Its **table** definition  **_must extend_** `ResourceOwnerTableDefinition`  and the instance type must implement  `ManagedAuthResourceOwner<T>`, where  `T`  is the table definition. A basic definition may look like this:

```dart
class User extends ManagedObject<_User>
    implements _User, ManagedAuthResourceOwner<_User> {
}
class _User extends ResourceOwnerTableDefinition {
  @Column(unique: true)
  String email;
}
```

By extending  **`ResourceOwnerTableDefinition`**  in the table definition, the database table has the following four columns:

-   an integer primary key named  `id`
-   a unique string  `username`
-   a password hash
-   a salt used to generate the password hash

A  `ResourceOwnerTableDefinition`  also has a  `ManagedSet`  of  `tokens`  for each token that has been granted on its behalf.

The interface  `ManagedAuthResourceOwner<T>`  is a requirement that ensures the type argument is both a  `ManagedObject<T>`  and  `ResourceOwnerTableDefinition`, and serves no other purpose than to restrict  `ManagedAuthDelegate<T>`'s type parameter.

This structure allows an application to declare its own 'user' type while still enforcing the needs of Aqueduct's OAuth 2.0 implementation.

The  `managed_auth`  library also declares two  `ManagedObject<T>`  subclasses.  `ManagedAuthToken`  represents instances of authorization tokens and codes, and  `ManagedAuthClient`  represents instances of OAuth 2.0 clients. This means that an Aqueduct application that uses  `ManagedAuthDelegate<T>`  has a minimum of three database tables: users, tokens and clients.

`ManagedAuthDelegate<T>`  will delete authorization tokens and codes when they are no longer in use. This is determined by how many tokens a resource owner has and the tokens expiration dates. Once a resource owner acquires more than 40 tokens/codes, the oldest tokens/codes (determined by expiration date) are deleted. Effectively, the resource owner is limited to 40 tokens. This number can be changed when instantiating  `ManagedAuthDelegate<T>`:


















































<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzNTMyMTkzLDEyMzM2MjI4NTksMTU4Mj
k5Mzg0MSwtNDE2OTQwMDk1LC0xNzkzNDQyNTEwLDE4MDQ4OTg1
OCwtMjcxNDg0MDkyLDE3NjA0MjI5Ml19
-->

## Understand Authentication:

following material fetched from [here](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html)

 ### Understanding Login Authentication

When you try to access a protected web resource, the web container activates the authentication mechanism that has been configured for that resource. You can specify the following authentication mechanisms:

-   HTTP basic authentication
-   Form-based login authentication
-   Client certificate authentication
-   Mutual authentication
-   Digest authentication

If you do not specify one of these mechanisms, the user will not be authenticated.

#### Using HTTP Basic Authentication

[Figure 32-2](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483341)  shows what happens if you specify  _HTTP basic authentication_.

![HTTP Basic Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-httpBasicAuthentication.gif)

Figure 32-2 HTTP Basic Authentication

With basic authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server returns a dialog box that requests the user name and password.
-   The client submits the user name and password to the server.
-   The server validates the credentials and, if successful, returns the requested resource.

HTTP basic authentication is **not** particularly secure. Basic authentication sends user names and passwords over the Internet as text that is uu-encoded (Unix-to-Unix encoded) but not encrypted. This form of authentication, which **uses Base64 encoding**, can expose your user names and passwords **unless all connections are over SSL**. If someone can intercept the transmission, the user name and password information can easily be decoded.

[Example: Basic Authentication with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp156943)  is an example application that uses HTTP basic authentication in a JAX-RPC service.

#### Using Form-Based Authentication

[Figure 32-3](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483393)  shows what happens if you specify  _form-based authentication_, in which you can customize the login screen and error pages that an HTTP browser presents to the end user.

![Form-Based Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-formBasedLogin.gif)

Figure 32-3 Form-Based Authentication

With form-based authentication, the following things occur:

-   A client requests access to a protected resource.
-   If the client is unauthenticated, the server redirects the client to a login page.
-   The client submits the login form to the server.
-   If the login succeeds, the server redirects the client to the resource. If the login fails, the client is redirected to an error page.

Form-based authentication is **not** particularly secure. In form-based authentication, the content of the user dialog box is sent as plain text, and the target server is not authenticated. This form of authentication can expose your user names and passwords **unless all connections are over SSL**. If someone can intercept the transmission, the user name and password information can easily be decoded.

[Example: Using Form-Based Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp303355)  is an example application that uses form-based authentication.



#### Using Client-Certificate Authentication

Client-certificate authentication is a more secure method of authentication than either basic or form-based authentication. It **uses HTTP over SSL**, in which the server and, optionally, the client **authenticate one another** using public key certificates.  Secure Socket Layer  (SSL) provides data encryption, server authentication, message integrity, and optional client authentication for a TCP/IP connection. **You can think of a  public key certificate  as the digital equivalent of a passport**. It is issued by a trusted organization, which is called a  certificate authority  (CA), and provides identification for the **bearer**.

If you specify  client-certificate authentication, the web server will authenticate the client using the client's  X.509 certificate, a public key certificate that conforms to a standard that is defined by X.509 Public Key Infrastructure (PKI). Before running an application that uses SSL, you must configure SSL support on the server (see  [Installing and Configuring SSL Support](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security6.html#wp509934)) and set up the public key certificate (see  [Understanding Digital Certificates](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security6.html#wp80737)).

[Example: Client-Certificate Authentication over HTTP/SSL with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp498398)  describes an example application that uses client-certificate authentication.



#### 📝 Using Mutual Authentication

With  _mutual authentication_ , the server and the client authenticate each other. There are two types of mutual authentication:

-   **Certificate-based** mutual authentication (see  [Figure 32-4](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp488092))
-   **User name- and password-based** mutual authentication (see  [Figure 32-5](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483607))


[Figure 32-4](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp488092)  shows what occurs during certificate-based mutual authentication.

![Certificate-Based Mutual Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-sslBasedMutualAuthenticationWithCertificates.gif)

⚡ Figure 32-4 **Certificate-Based** Mutual Authentication

In certificate-based mutual authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server presents its certificate to the client.
-   The client verifies the server's certificate.
-   If successful, the client **sends its certificate** to the server.
-   The server verifies the client's credentials.
-   If successful, the server grants access to the protected resource requested by the client.

[Example: Client-Certificate Authentication over HTTP/SSL with JAX-RPC](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security7.html#wp498398)  describes an example application that uses certificate-based mutual authentication.

[Figure 32-5](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/Security5.html#wp483607)  shows what occurs during user name- and password-based mutual authentication.

![Username/Password-Based Mutual Authentication](http://lia.deis.unibo.it/Courses/TecnologieWeb0708/materiale/laboratorio/guide/j2ee14tutorial7/images/security-sslBasedMutualAuthenticationWithUsernamePassword.gif)

⚡ Figure 32-5 User Name- and Password-Based Mutual Authentication

In user name- and password-based mutual authentication, the following things occur:

-   A client requests access to a protected resource.
-   The web server presents its certificate to the client.
-   The client verifies the server's certificate.
-   If successful, the client sends **its user name and password** to the server, which verifies the client's credentials.
-   If the verification is successful, the server grants access to the protected resource requested by the client.



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI2MTcyMTg0M119
-->

## Codecs:

Not all content types require two conversion steps. For example, when serving an HTML file, the body object is already an HTML  `String`. It will only be converted by a charset encoder:
```dart
var html = "<html></html>";
var response = Response.ok(html)
  ..contentType = ContentType.html;
```
#### Image Codec
An image body object needs no conversion at all, since it is already a list of bytes. **If** there is **no registered codec** for a content-type, the body object **must be a byte array** (`List<int>`  where each value is between 0-255).
```dart
final imageFile = File("image.jpg");
final imageBytes = await imageFile.readAsBytes();
final response = Response.ok(imageBytes)
  ..contentType = ContentType("image", "jpeg");
```

##### disable automatic encoding

You may **disable** the **automatic encoding** of a body as long as the body object is a **byte** array:
```dart
final jsonBytes = utf8.encode(json.encode({"key": "value"}));
final response = Response.ok(jsonBytes)..encodeBody = false;
```
See a later section for more details on content type to codec mappings. Also, see the documentation for  `CodecRegistry`  for details on built-in codecs and adding codecs.


### Streaming Response Bodies

A **body** object **may also be a  `Stream<T>`**.  `Stream<T>`  body objects are most **often used when serving files**. This allows the contents of the file to be **streamed from disk** to the HTTP client **without** having to load the whole file into memory first. (See also  `FileController`.)
```dart
final imageFile = File("image.jpg");
final imageByteStream = imageFile.openRead();
final response = new Response.ok(imageByteStream)
  ..contentType = new ContentType("image", "jpeg");
```
> ⚡ When a body object is a  `Stream<T>`, the response will not be sent until the stream is closed. 

> For finite streams - like those from opened filed - this happens as soon as the entire file is read. For streams that you construct yourself, you must close the stream some time after the response has been returned.

## Codecs and Content Types

In the above sections, we glossed over how a codec gets selected when preparing the response body. The common case of  `ManagedObject<T>`  body objects that are sent as UTF8 encoded JSON 'just works' and is suitable for most applications. When serving assets for a web application or different data formats like XML, it becomes important to understand how Aqueduct's codec registry works.

> ⚡ `CodecRegistry`  contains mappings from content types to  `Codec`s. These codecs **encode response** bodies and **decode request** bodies. 
There are three built-in codecs for 
  - `application/json`, 
  - `application/x-www-form-urlencoded`  and  
  - `text/*`. 

> ⚡ When a response is being sent, the repository is searched for an entry that exactly matches the primary and subtype of the  `Response.contentType`. If an entry exists, the associated  `Codec`  starts the conversion. 

For example, if the content type is  `application/json; charset=utf-8`, the built-in  `application/json`  codec encodes the body object.

> ⚡ If there isn't an exact match, but there is an entry for the primary type with the wildcard (`*`) subtype, that codec is used. For example, the built-in codec for  `text/*`will be selected for both  `text/plain`  and  `text/html`. 

### register codecs into CodeRegistery

If there was something special that had to be done for  `text/html`, a more specific codec may be added for that type:
```dart
class MyChannel extends ApplicationChannel {
  @override
  Future prepare() async {
    CodecRegistry.defaultInstance.add(ContentType("application", "html"), HTMLCodec());
  }
}
```
> ⚡ Codecs must be added in your  `ApplicationChannel.prepare`  method. **The codec must implement  `Codec`  from  `dart:convert`**. In the above example, when a response's content type is  `text/html`, the  `HTMLCodec`  will encode the body object. This codec takes precedence over  `text/*`  because it is more specific.

> 📓 When selecting a codec for a response body, the  `ContentType.charset`  doesn't impact which codec is selected. 

If a response's content-type has a charset, then a charset encoder like  `UTF8`  will be applied as a last encoding step. For example, a response with content-type  `application/json; charset=utf-8`  will encode the body object as a JSON string, which is then encoded as a list of UTF8 bytes. It is required that a response body's eventually encoded type is a list of bytes, so it follows that a codec that produces a string must have a charset.

> ⚡ If there is no codec in the repository for the content type of a  `Response`, the body object must be a  `List<int>`  or  `Stream<List<int>>`. 
>  📘 If you find yourself converting data prior to setting it as a body object, it may make sense to add your own codec to  `CodecRegistry`.

> ⚡ A request's body always starts as a **list of bytes** and is decoded into Dart objects. To decode a JSON request body, it first must be decoded from the **list of UTF8** bytes into a **string**. 

#### register JsonCodec into CodecRegistry

It is possible that a client could omit the charset in its content-type header. Codecs added to  `CodecRegistry`  may specify a default charset to interpret a charset-less content-type. When a codec is added to the repository, if content-type's charset is non-null, that is the default. For example, the JSON codec is added like this:
```dart
CodecRegistry.defaultInstance.add(
  ContentType("application", "json", charset: "utf-8"),
  const JsonCodec(),
  allowCompression: true);
```
If no charset is specified when registering a codec, no charset decoding occurs on a request body if one  doesn't exist. Content-types that are decoded from a  `String`should not use a default charset because the repository would always attempt to decode the body as a string first.

### Compression with gzip

Body objects may be compressed with  `gzip`  if the HTTP client allows it  _and_  the  `CodecRegistry`  has been configured to compress the content type of the response. 

> ⚡ The three built-in codecs 
> -  `application/json`
> -   `application/x-www-form-urlencoded`  and  
> - `text/*`  
are all configured to allow compression. Compression occurs as the last step of conversion and only if the HTTP client sends the  `Accept-Encoding: gzip`  header.

Content types that are not in the codec repository will not trigger compression, even if the HTTP client allows compression with the  `Accept-Encoding`  header. This is to prevent binary contents like images from being 'compressed', since they are likely already compressed by a content-specific algorithm. 

> ⚡ In order for Aqueduct to compress a content type other than the built-in types, you may add a codec to the repository with the  `allowCompression`  flag. (The default value is  `true`.)
```dart
CodecRegistry.add(
  ContentType("application", "x-special"),
   MyCodec(),
  allowCompression: true);
```
You may also set whether or not a content type uses compression without having to specify a codec if no conversion step needs to occur:
```dart
CodecRegistry.setAllowsCompression(new ContentType("application", "x-special"), true);
```

## Serializable Objects

Encoding and decoding primarily takes or yields simple Dart objects like  `Map`,  `List`and  `String`. It is often beneficial to add structure to body objects for use in your code. Classes that implement  `Serializable`  provide this structure. A  `Serializable`  object must implement a  `readFromMap()`  and  `asMap()`  method to convert their structure into or from a Dart  `Map`.

> ⚡ An object that extends  `Serializable`  may be used as a response body object directly:

```dart
class Person extends Serializable {
  String name;
  String email;
  Map<String, dynamic> asMap() {
    return {
      "name": name,
      "email": email
    };
  }
  void readFromMap(Map<String, dynamic> inputMap) {
    name = inputMap['name'];
    email = inputMap['email'];
  }
}

final person = Person();
final response = Response.ok(person);
```

> ⚡ When responding with a  `Serializable`, its  **`asMap()`**  is called **prior** to any **encoding** by the codec registry.  `ManagedObject<T>`, part of the Aqueduct ORM, implements  `Serializable`  so results from  `Query<T>`  may be body objects:
```dart
final query = Query<Person>(context)..where((p) => p.id).equalTo(1);
final person = await query.fetchOne();
final response = Response.ok(person);
```
A response body object can also be a list of  `Serializable`  objects.
```dart
final query = Query<Person>(context);
final people = await query.fetch();
final response = Response.ok(people);
```
The entire flow of a body object is shown in the following diagram. Each orange item is an allowed body object type and shows the steps it will go through when being encoded to the HTTP response body. For example, a  `Serializable`  goes through three steps, whereas a  `List<int>`  goes through zero steps and is added as-is to the HTTP response.

![Response Body Object Flow](https://aqueduct.io/docs/img/response_flow.png)

A serializable object can be read from a request body:
```dart
final person = Person()..readFromMap(await request.body.decode());
```

> ⚡ **Serializable** objects are the only types of objects that can be  [bound to a ResourceController argument](https://aqueduct.io/docs/http/resource_controller/).

```dart
@Operation.post()
Future<Response> addPerson(@Bind.body() Person person) async {
  final insertedPerson = await context.insertObject(person);
  return Response.ok(insertedPerson);
}
```
### Serializable and OpenAPI Generation

See the section on how  `Serializable`  types work with OpenAPI documentation generation  [here](https://aqueduct.io/docs/openapi/components/).

## 🔥 Example: multipart/form-data
**Add  `package:mime`  to your  `pubspec.yaml`  as a dependency.**
```
dependencies:
  mime: any # prefer a better constraint than this
```

And then decode the body with the objects from that package:

```dart
import 'package:aqueduct/aqueduct.dart';
import 'package:mime/mime.dart';

class MyController extends ResourceController {
  MyController() {
    acceptedContentTypes = [ContentType("multipart", "form-data")];
  }

  @Operation.post()
  Future<Response> postForm() async {}
    final transformer = MimeMultipartTransformer(request.raw.headers.contentType.parameters["boundary"]);
    final bodyStream = Stream.fromIterable([await request.body.decode<List<int>>()]);
    final parts = await transformer.bind(bodyStream).toList();

    for (var part in parts) {
      final headers = part.headers;
      final content = await part.toList();
      ...
    }    
  }
}
```
> 📘 Further explanation
>  **MimeMultipartTransformer**
> >Parser for MIME multipart types of data as described in RFC 2046 section 5.1.1. The data is transformed into [MimeMultipart](https://pub.dartlang.org/documentation/mime/latest/mime/MimeMultipart-class.html) objects, each of them streaming the multipart data.
	
> **methods**
> **bind**(**Stream<List<int>>** stream) → **Stream**<**MimeMultipart**>
> 
> > Transforms the provided  `stream`. Returns a new stream with events that are computed from events of the provided  `stream`.



## Processing MIME multipart media types  [#](https://pub.dartlang.org/packages/mime#processing-mime-multipart-media-types)

The class  **`MimeMultipartTransformer`**  is used to process a  **`Stream`**  of bytes encoded using a **MIME multipart media types encoding**. The transformer provides a new  `Stream`  of  `MimeMultipart`  objects each of which have the headers and the content of each part. The content of a part is provided as a stream of bytes.

> 📘 上述 The transformer provides a new  `Stream` .... and the content of each part. 指的是multipart header 中每個被boundary分割的區塊

Below is an example showing how to process an HTTP request and print the length of the content of each part.
```dart
// HTTP request with content type multipart/form-data.
HttpRequest request = ...;
// Determine the boundary form the content type header
String boundary = request.headers.contentType.parameters['boundary'];
// Process the body just calculating the length of each part.
request
    .transform(new MimeMultipartTransformer(boundary))
    .map((part) => part.fold(0, (p, d) => p + d))
    .listen((length) => print('Part with length $length'));
```



> #### How to Send Form Data in Dart
> 
```dart
		FormData data = new FormData(); // from dart:html
		data.append(key, value);
		HttpRequest.request('/upload', method: 'POST', sendData: data).then((HttpRequest r) 
```

> #### Send Streaming Data via FormData by powerful Http client, dio
> 
```dart
FormData formData = new FormData.from({
    "name": "wendux",
    "age": 25,
    "file": new UploadFileInfo(new File("./example/upload.txt"), "upload.txt")
});
response = await dio.post("/info", data: formData);
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyMjAwNjQxMSw3NzMzMjk0NTYsLTEwOT
Y3MTI1MDUsNTQwNjYzNDg1LDEyNDY2MDczNCwtMTExNDg3MTY3
LDE3NTgwNDAwNTgsLTUwNTMxNDEwMF19
-->

## Core Concept:

## Core Concepts

### Resources
Resources are the things your application exposes through its HTTP API.
In HTTP, resource means data which is the one being transferred and manipulated.

-   GET — Get a list of resources or single resource
-   POST — Create new resource
-   PUT — Update existing resource
-   DELETE — Delete a resource

### Routing
The route /organizations/:id will match the paths /organizations/1, /organizations/2, and so on.



### Controllers
Controllers are objects that handle requests. For example, a controller might fetch rows from a database and send them to the client in the response body. Another controller might verify the username and password of a request's Authorization header are valid. **Controllers are linked together to form a series of actions to take for a request. These linked together controllers are called a _channel_.**

There are two flavors of controllers. 
- #### _endpoint controller_  
	 performs operations on a resource or resource collection, and always sends a response. Endpoint controllers  _fulfill_  requests by returning the state of a resource or by changing the state of a resource. You write most of your application-specific logic in endpoint controllers.

- #### _middleware controller_  
	takes an action for a request, but isn't responsible for fulfilling the request. Middleware controllers can do many different things and are often reusable in many channels. Most often, a middleware controller validates something about a request before it reaches an endpoint controller. Middleware controllers can send a response for a request, and doing so prevents any other controller in that channel from handling the request.

A channel must have exactly one endpoint controller. It can be preceded by zero or more middleware controllers. See the guides on  [Controllers](https://aqueduct.io/docs/http/controller/)  and  [ResourceControllers](https://aqueduct.io/docs/http/resource_controller/)  for usage details.



#### @Operation.get 

**_a member of ResourceController_**
- **implement handle method automatically**
we didn't have to override  **`handle`**  in  `ResourceController`. A  `ResourceController`  implements this method to call one of our  **_operation** methods_. An operation method - like  `getAllHeroes`  and  `getHeroByID`  - must have an  **`Operation`** annotation. The named constructor  **`Operation.get`**  means these methods get called when the request's method is **GET**. An operation method must also return a  `**Future<Response>**`.

`getHeroByID`'s annotation also has an argument - the name of our path variable  `id`. If that path variable exists in the request's path,  `getHeroByID`  will be called. If it doesn't exist,

```dart
class HeroesController extends ResourceController {
  final _heroes = [
    {'id': 11, 'name': 'Mr. Nice'},
    {'id': 12, 'name': 'Narco'},
    {'id': 13, 'name': 'Bombasto'},
    {'id': 14, 'name': 'Celeritas'},
    {'id': 15, 'name': 'Magneta'},
  ];
  @Operation.get()
  Future<Response> getAllHeroes() async {
    return Response.ok(_heroes);
  }
  @Operation.get('id')
  Future<Response> getHeroByID() async {
    final id = int.parse(request.path.variables['id']);
    final hero = _heroes.firstWhere((hero) => hero['id'] == id, orElse: () => null);
    if (hero == null) {
      return Response.notFound();
    }
    return Response.ok(hero);
  }
}
```

### Request Binding @bind

In our  `getHeroByID`  method, we make a dangerous assumption that the path variable 'id' can be parsed into an integer. If 'id' were something else, like a string,  `int.parse`  would throw an exception. When exceptions are thrown in operation methods, the controller catches it and sends a 500 Server Error response. 500s are bad, they don't tell the client what's wrong. A 404 Not Found is a better response here, but writing the code to catch that exception and create this response is cumbersome.

Instead, we can rely on a feature of operation methods called  _request binding_. An operation method can declare parameters and  _bind_  them to properties of the request.

```dart
@Operation.get('id')
Future<Response> getHeroByID(@Bind.path('id') int id) async {
  final hero = _heroes.firstWhere((hero) => hero['id'] == id, orElse: () => null);

  if (hero == null) {
    return Response.notFound();
  }
  return Response.ok(hero);
}
```


### Using ManagedContext to Connect to a Database


The interface to a database from Aqueduct is an instance of  `ManagedContext`  that contains the following two objects:

-   a  `ManagedDataModel`  that describes your application's data model
-   a  `PersistentStore`  that manages a connection to a single database

A **`ManagedContext`** uses these two objects to coordinate moving data to and from your application and a database. A **`Query<T>`** object uses a context's persistent store to determine **which database to send commands to**, and a **data model to map database rows to objects** and vice versa.

A context, like all service objects, is created in **`ApplicationChannel.prepare`**.

```dart
class MyApplicationChannel extends ApplicationChannel {
  ManagedContext context;

  @override
  Future prepare() async {
    var dataModel = ManagedDataModel.fromCurrentMirrorSystem();
    var psc = PostgreSQLPersistentStore.fromConnectionInfo(
   "my_app_name_user", "password", "localhost", 5432, "my_app_name");

    context = ManagedContext(dataModel, psc);
  }
}
```

The `ManagedDataModel.fromCurrentMirrorSystem` finds every **`ManagedObject<T>`** subclass in your application's code. Optionally, you may specify an exact list:

```dart
var dataModel = ManagedDataModel([User, Post, Friendship]);
```


### Configuration Files

following  `Configuration`  subclass:

```dart
class TodoConfiguration extends Configuration {
  TodoConfiguration(String fileName) : super.fromFile(fileName);

  DatabaseConnectionConfiguration database;
  String apiBaseURL;

  @optionalConfiguration
  int identifier;
}
```

This would read a YAML file like this:

```yaml
database:
  username: fred
  password: fredspassword
  host: db.myapp.com
  port: 5432
  databaseName: fredsdb
apiBaseURL: /api
identifier: 2
```

If required properties are omitted from the YAML file being read, application startup will fail and throw an informative error.

You may use `Configuration`s to read values from environment variables. In `config.yaml`, use a **`$`-prefixed** ***environment variable*** name instead of a value:

```yaml
database: $DATABASE_CONNECTION_URL
apiBaseURL: /api
```
> $: 指向 ENV variable

 

#### Configuration Conventions and Deployment Options

Aqueduct uses two configuration files for a project: `config.yaml` and `config.src.yaml`. 

- The latter is the _configuration source file_. 
  The configuration source file declares key-value pairs that will be used when running the application tests. Deployed instances use `config.yaml`.

This pattern is used for two reasons:

-   It is the template for the  `config.yaml`  that will be read on deployed applications, providing documentation for your application's configuration.
-   It has the configuration values used during testing to inject mock dependencies.

For example, a production API instance might have the following  `config.yaml`  file with connection info for a production database:

```yaml
database: postgres://app_user:$%4jlkn#an*@mOZkea2@somedns.name.com:5432/production_db
```

Whereas  `config.src.yaml`  would have connection info for a local, test database:

```yaml
database: postgres://test:test@localhost:5432/temporary_db
```










<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNzM4MDc3NDAsLTgwNTIxOTg0NiwzNT
g5OTY5MzEsLTE2OTY5MzM4MTEsMTU2MjU0NTA0MSwxMjQxMTE2
Mjg1LC00MTA0ODA0OSwtMzE4MTE1NjYxLDQ3Mjk5OTQxMSw2Mj
AwODIyMzgsLTg1MjA1NjU0NCwtNjcxNjkzMzIzLC0yMDE2NzEz
NDAxLC00NDM2NDY3OCwtMTE4OTg0OTA1LDUzODkyNDcxMiwxNz
kzMjQyODU3LC01Njg1Mzk4NSwyMDg2MDA4ODA0LDE1NTkzMTEy
NDFdfQ==
-->

## DataBase:



## Example: One-to-Many Relationship

#### An author has many books:
```dart
class Author extends ManagedObject<_Author> implements _Author {}
class _Author {
  @primaryKey
  int id;

  String name;

  ManagedSet<Book> books;
}

class Book extends ManagedObject<_Book> implements _Book {}
class _Book {
  @primaryKey
  int id;

  String name;

  @Relate(#books)
  Author author;
}
```

> #### ⚡ @Relate 關連欄位
> 需要在table 插入時一併寫入該欄位，否則會出現錯誤
> #### ⚡ ManagedSet<ReferencedType>
> 此為Query指向用,無實際欄位, 不得插入 


#### To insert an author and a book associated with that author:
```dart
final authorQuery = Query<Author>(context)
  ..values.name = "Fred";
final author = await authorQuery.insert();

final bookQuery = Query<Book>(context)
  ..values.name = "Title"
  ..values.author.id = author.id;
final book = await bookQuery.insert();
```
#### To fetch authors and their books:
```dart
final query = Query<Author>(context)
  ..join(set: (a) => a.books);
final authors = await query.fetch();
```

> ⚡ **set:** indicates **ManagedSet** which is an list of int referenced to a target table

#### To fetch a book and their full author object:
```dart
final query = Query<Book>(context)
  ..where((b) => b.id).equalTo(1)
  ..join(object: (a) => a.author);
final books = await query.fetch();
```

> ⚡ **object:** indicates a **Relate** object which referenced from author



### Example: Hierarchical Relationships (Self Referencing)

Hierarchical relationships follow the same rules as all other relationship, but declare the foreign key property and the inverse in the same type.
```dart
class Person extends ManagedObject<_Person> implements _Person {}
class _Person {
  @primaryKey
  int id;

  String name;

  ManagedSet<Person> children;

  @Relate(#children)
  Person parent;
}
```



## Example: Many-to-Many Relationship

Hierarchical relationships follow the same rules as all other relationship, but declare the foreign key property and the inverse in the same type.

```dart
class Team extends ManagedObject<_Team> implements _Team {}
class _Team {
  @primaryKey
  int id;

  String name;
  ManagedSet<TeamPlayer> teamPlayers;
}

// This type is a join table
class _TeamPlayer extends ManagedObject<_TeamPlayer> implements _TeamPlayer {}
class _TeamPlayer {
  @primaryKey
  int id;  

  @Relate(#teamPlayers)
  Team team;

  @Relate(#teamPlayers)
  Player player;
}

class Player extends ManagedObject<_Player> implements _Player {}
class _Player {
  @primaryKey
  int id;

  String name;
  ManagedSet<TeamPlayer> teamPlayers;
}
```



## ManagedObject Serialization and Deserialization

In this guide, you will learn how  `ManagedObject<T>`s are read from HTTP request bodies and written to HTTP response bodies.



## Basic Behavior

A  `ManagedObject<T>`  can be converted to and from  `Map<String, dynamic>`  objects (which can be encoded and decoded into request or response body using JSON or some other data format). 

- **`readFromMap`:**
  To decode a  `ManagedObject`  into a  `Map`, use the instance method  
	```dart
	final object = MyManagedObject();
	object.readFromMap({
	  "key": "value"
	});
	```
	When decoding, the value for each key in the map is assigned to the managed object property of the same name. 

	> ⚡ If a key exists in the map and the managed object does not have a property of the same name, a `ValidationException` will be thrown (this sends a **400 Bad Request response if uncaught**).


- **`asMap`:**
  To encode a map into a managed object is converted, use the instance method  
	```dart
	final object = MyManagedObject();
	final map = object.asMap();
	```
	
|dart type| serialized type |
|--|--|
| int | number(int) |
| double| number(double) |
| String| string(String) |
| DateTime| ISO 8601 Timestamp (`String`) |
| bool | boolean(bool) |
| Document | map or list (`Map<String, dynamic>` or `List<dynamic>`) |
| enum |string(String) |
| RelationShip | list or maps |
 
Both  `asMap`  and  `readFromMap`  are inherited methods from  `Serializable`. As a  `Serializable`, a managed object can be  [bound to a request body in an  `ResourceController`  operation method](https://aqueduct.io/docs/http/resource_controller/)  and  [encoded as a response body object](https://aqueduct.io/docs/http/request_and_response/). For example:

```dart
class UserController extends ResourceController {
  @Operation.post()
  Future<Response> createUser(@Bind.body() User user) async {
    var query = Query<User>(context)
      ..values = user;

    final newUser = await query.insert();

    return Response.ok(newUser);
  }
}
```
> ⚡ **@Bind.body()**
> Binds an HTTP request body to an [ResourceController] property or operation method argument.
> _

> ⚡ **Autoincrementing Properties**
> Properties that are autoincrementing will never be read from a map from `readFromMap`.
> _


## Behavior of Null Values

A property of a **managed** object **can be null for two reasons:** 
	- the value is actually null
	- or the value is not available
For example, when you create a new instance of a managed object, none of its values are available (the object is empty). When encoding an object into a map, only the available values are included and the keys for any unavailable properties are omitted:


## Behavior of Transient Properties

By default, transient properties - those declared in the **managed** object subclass, **not the table definition** - are  **_not_**  included in an object's  `asMap()`. The  **`Serialize`**  annotation **allows** a transient property to be included in this map.

Properties declares in a managed object subclass are called  _transient_  because they are not stored in a database. For example, consider an  `Author`  type that stores first and last name as separate columns. Instead of redundantly storing a 'full name' in the database

```dart
class Author extends ManagedObject<_Author> implements _Author {
  String get name => "$firstName  $lastName";
  set name(String fullName) {
    firstName = fullName.split(" ").first;
    lastName = fullName.split(" ").last;
  }
}
```

```dart
class Author extends ManagedObject<_Author> implements _Author {
  @Serialize()
  String get name => "$firstName  $lastName";

  @Serialize()
  set name(String fullName) {
    firstName = fullName.split(" ").first;
    lastName = fullName.split(" ").last;
  }
}
```
> ⚡ Getters with the `Serialize` annotation will be written in `asMap` and setters with the annotation will be read in `readFromMap`.

> ⚡ You can annotate a transient property with  `Serialize`   so that it is able to be **read from a request body**, **written to a response body**, or both. For example:

```dart
class Employee extends ManagedObject<_Employee> implements _Employee {
  int a; // NOT included in asMap, NOT read in readFromMap
  @Serialize() int b; // included in asMap, read in readFromMap
  @Serialize(input: true, output: false) int c; // NOT included in asMap, read in readFromMap
  @Serialize(input: false, output: true) int d; // included in asMap, NOT read in readFromMap
}
class _Employee {
  @primaryKey
  int id;
}
```

 
## Auth
 
```dart
class _User extends ResourceOwnerTableDefinition {  
  @Column(unique: true)  
  String email;  
  ManagedSet<Note> notes;  
}
class User extends ManagedObject<_User> implements _User, ManagedAuthResourceOwner<_User> {  
  @Serialize(input: true, output: false)  
  String password;
}
class RegisterController extends QueryController<User> {  
  RegisterController(ManagedContext context, this.authServer) : super(context);  
  final AuthServer authServer;  
  
  @Operation.post()  
  Future<Response> createUser() async {  
    if (query.values.username == null || query.values.password == null) {  
      return Response.badRequest(body: {"error": "username and password required."});  
  }  
  
  query.values.username = query.values.username.toLowerCase();  
  
  final salt = AuthUtility.generateRandomSalt();  
  final hashedPassword = AuthUtility.generatePasswordHash(query.values.password, salt);  
  
  query.values.hashedPassword = hashedPassword;  
  query.values.salt = salt;  
  query.values.email = query.values.username;  
  
  final u = await query.insert();  
  final token = await authServer.authenticate(  
      u.username,  
  query.values.password,  
  request.authorization.credentials.username,  
  request.authorization.credentials.password);  
  
  return AuthController.tokenResponse(token);  
  }  
}
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTkzMjgzNTcsMTUyNDg4MjU5MSwtMT
c2MDYyNDE0MywtMTIyNjAwOTU1NywtMTUzNDcyMDA4NSw4NDE3
OTQzMTQsLTEyODc0NjA5ODAsOTM1MTY1NzY3LDE5NTQ5Nzg4NT
csMzQ0MDUxODk5LDY4MzMyMjg3MiwtMTcwMTE0MjU5NCwtMTAz
NTMxNjY5MiwyMDcwMjI3NDA4LDEzNzI3NTg2OTYsMTI0NDA1NT
gxLDIwODk3MjY0MzRdfQ==
-->

## JSON:
## JSON Document Storage

Learn how to store unstructured, binary JSON data in  `ManagedObject<T>`properties.

### JSON Columns in Relational Databases

PostgreSQL supports many column data types like integers, strings, booleans and dates. A column may also be JSON data. This allows for **storing unstructured data** and **simple objects** in a table column. The data from JSON columns can be fetched all at once, or in pieces. Elements of JSN data can be used to filter the results of a query.

### The Document Data Type

JSON document columns are added to a database table by declaring a  **`Document`property** in a  `ManagedObject<T>`'s table definition. In PostgreSQL, a  **`Document`**column data type is  **`jsonb`**. A document column can only contain **JSON-encodable** data. This data is typically a  **`Map`**  or  **`List`**  that contains only **JSON-encodable** data. The following  `ManagedObject<T>`  declaration will have a  `contents`  column of type  `jsonb`.
```dart
class Event extends ManagedObject<_Event> implements _Event {}
class _Event {
  @primaryKey
  int id;
  @Column(indexed: true)
  DateTime timestamp;
  Document contents;
}
```

> ⚡ A  **`Document`**  object has a  **`data`**  property to hold its **JSON-encodable** data. When instantiating  `Document`, this property **defaults to null unless** a value has been provided to the optional, ordered parameter in its constructor.
```dart
final doc = new Document();
assert(doc.data == null);

final doc = new Document({"key": "value"});
assert(doc.data is Map);

final doc = new Document([0]);
assert(doc.data is List);
```
The data in a document can be accessed through its  `data`  property, or through its subscript operator.  `Document`'s subscript operator forwards the invocation to its  `data`  property.
```dart
final doc = new Document({"key": "value"});
assert(doc["key"] == doc.data["key"]);
```
The argument to the subscript operator may be a string (if  `data`  is a map) or an integer (if  `data`  is a list).

### Basic Operations on Document Properties

`Document`  columns are like any other type of column, and can therefore be set during an insert or update, and read during a fetch.

#### Inserting Rows with Document Properties

A  **`Document`**  property is first set **when inserting** with a  `Query<T>`. The  `values`property of the query is set to a  `Document`  object initialized with a JSON-encodable value.
```dart
final query = Query<Event>(context)
  ..values.timestamp = DateTime.now()
  ..values.contents = Document({
    "type": "push",
    "user": "bob",
    "tags": ["v1"]
  });
final event = await query.insert();  
```
In the above, the argument to  `Document`  will be JSON-encoded and stored in the database for column  `contents`. If the object can't be encoded as JSON, an exception will be thrown.

#### Fetching Rows with Document Properties

> ⚡ When fetching an object with  `Document`  properties with a  `Query<T>`, you **access the column's value through the document's  `data`**  property.
```dart
final query = Query<Event>(context)
  ..where((e) => e.id).equalTo(1);
final event1 = await query.fetchOne();
event1.contents.data == {
  "type": "push",
  "user": "bob",
  "tags": ["v1"]
};
```
When fetching  `Document`  properties, the JSON data is decoded into the appropriate type. This is likely a  `Map`  or  `List`, but can be any JSON-encodable object. 

> ⚡ Because the data stored in a  `Document`  property is unstructured, the type of  `data`  is  `dynamic`. It is good practice to **store consistent data structures** in a column; i.e., always storing a  `Map`  or always storing a  `List`.

#### Updating Rows with Document Properties

Updating a row with  `Document`  properties works the same as inserting rows.
```dart
final query = Query<Event>(context)
  ..where((e) => e.id).equalTo(1)
  ..values.contents = Document({
    "type": "push",
    "user": "bob",
    "tags": ["v1", "new"]
  });
final event = await query.updateOne();  
```
When updating in this way, the document stored in the column is replaced entirely.

#### Accessing Document Values

**The type of  `Document.data`  is  `dynamic**`  - it can be any valid JSON type and may be casted to the expected type when used. This data can also be nested - a  `List`of  `Maps`, for example. When accessing object keys or list indices, you may use the subscript operator directly on  `Document`.

```dart
// Object Access by key
final doc = Document({"key": "value"});
final value = doc["key"] == "value";
// List Access by index
final doc = Document(["v1", "v2"]);
final value = doc[0] == "v1";
```
You can access nested elements with the same syntax:

```dart
final doc = Document([
  {"id": 1},
  {"id": 2}
]);
final obj1 = doc[0]["id"]; // == 1
final obj2 = doc[1]["id"]; // == 2
```
> ⚡ Note that **using the subscript operator on a  `Document`  simply invokes it on its  `data`  property**. Therefore, any subscript values must be valid for Dart  `List`  and  `Map`  types.

### Fetching Sub-documents

> ⚡ When fetching a  `Document`  property, the default behavior is to return the entire JSON document as it is stored in the database column. **You may fetch parts of the document you need** by using  **`Query.returningProperties`**  and the subscript operator.
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"]]);
final eventsWithTags = query.fetch();
```
When using the subscript operator on a returned  `Document`  property, only the value for that key is returned. For example, if the above query were executed and the stored column's value were:
```json
{
  "type": "push",  
  "user": "bob",
  "tags": ["v1"]  
}
```
The value of  `Event.contents`  would only contain the array for the key "tags":
```json
["v1"]
```
You may also index arrays in a JSON column using the same subscript operator, and the subscript operator can also be nested. For example, the following query would fetch the "tags" array, and then fetch the string at index 0 from it:
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][0]]);
final eventsWithFirstTag = await query.fetchOne();
eventsWithFirstTag.contents.data == "v1";
```
If a key or index does not exist in the JSON document, the value of the returned property will be null. For this reason, you should use null-aware operators when accessing  `Document.data`:
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][7]]); // 7 is out of bounds
final eventsWithFirstTag = await query.fetchOne();
if (eventsWithFirstTag.contents?.data == "v1") {
  ...
}
```
When fetching elements from a JSON array, you may use negative indices to specify a index from the end of the array.
```dart
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][-1]]);
final eventsWithLastTag = await query.fetchOne();
```
Note that you can only fetch a single sub-structure from a  `Document`  column per query. That is, you may not do the following:
```dart
// Invalid
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["type"], e.contents["user"]]);
```
For operations not supported by  `Query<T>`, you may use SQL directly:
final eventTagCounts = await context.persistentStore.execute("SELECT jsonb_array_length(contents->'tags') from _Event");













<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg0MzIwMTE1OSwtNjU4NzI4NzA0LDQ5MT
A3MDc4MV19
-->

## Multipart:
### Multipart header
#### [What should a Multipart HTTP request with multiple files look like? [duplicate]](https://stackoverflow.com/questions/913626/what-should-a-multipart-http-request-with-multiple-files-look-like)

#### There are  [three possibilities](https://www.w3.org/TR/html5/sec-forms.html#element-attrdef-form-enctype)  for  `enctype`:

-   [`application/x-www-form-urlencoded`](https://www.w3.org/TR/html5/sec-forms.html#urlencoded-form-data)
-   [`multipart/form-data`](https://www.w3.org/TR/html5/sec-forms.html#multipart-form-data)  (spec points to  [RFC7578](https://tools.ietf.org/html/rfc7578))
-   [`text/plain`](https://www.w3.org/TR/html5/sec-forms.html#plain-text-form-data). This is "not reliably interpretable by computer", so it should never be used in production, and we will not look further into it.

#### When you are writing client-side code:

-   use  **`multipart/form-data`**  when your form includes any  `<input type="file">`  elements
-   otherwise you can use  **`multipart/form-data`**  or  **`application/x-www-form-urlencoded`**  but  **`application/x-www-form-urlencoded`**  will be more efficient

	`enctype='multipart/form-data`  is an encoding type that allows files to be sent through a  _POST_. Quite simply, without this encoding the files cannot be sent through  _POST_. If you want to allow a user to upload a file via a form, you must use this  _enctype_.

_what a multipart header looks like_
```http
POST / HTTP/1.1
[[ Less interesting headers ... ]]
Content-Type: multipart/form-data; boundary=---------------------------735323031399963166993862150
Content-Length: 834

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="text1"

text default
-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="text2"

aωb
-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file1"; filename="a.txt"
Content-Type: text/plain

Content of a.txt.

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file2"; filename="a.html"
Content-Type: text/html

<!DOCTYPE html><title>Content of a.html.</title>

-----------------------------735323031399963166993862150
Content-Disposition: form-data; name="file3"; filename="binary"
Content-Type: application/octet-stream

aωb
-----------------------------735323031399963166993862150--

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzkxMTIyOTQsMTY5MDg3OTM1MywtODc0OT
M4MjYzXX0=
-->

 ## Resource Controller:
## ResourceController

A  `ResourceController`  is a  [controller](https://aqueduct.io/docs/http/controller/)  that provide conveniences for **implementing endpoint controllers**. A  `ResourceController`  must be subclassed, and in that subclass, you write a method for each operation on that type of resource. For example, a  `UserController`  might handle the following operations:

-   creating a new user (`POST /users`)
-   getting all users (`GET /users`)
-   getting an individual user (`GET /users/:id`)
-   updating an individual user (`PUT /users/:id`)
-   deleting an individual user (`DELETE /users/:id`)

These methods that are invoked for an operation are called  _operation methods_.

### Operation Methods

> ⚡ An **operation method** is an instance method of a  **`ResourceController`**  subclass that has an  **`@Operation`**  annotation. It must return an instance of  **`Future<Response>`**. Here's an example:

```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities() async {
    return new Response.ok(["Atlanta", "Madison", "Mountain View"]);
  }
}
```

The above operation method will be invoked when  `CityController`  **handles  `GET`**  requests **without** path variables. To handle operation methods **with** path variables, the name of the path variable is added to the  `@Operation`  annotation:

```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities() async {
    return new Response.ok(["Atlanta", "Madison", "Mountain View"]);
  }
  @Operation.get('name')
  Future<Response> getCityByName() async {
    final id = request.path.variables['name'];
    return new Response.ok(fetchCityWithName(name));
  }
}
```

##### Path Variables

This controller would be linked to the route specification  `/cities/[:name]`, so that it can handle both of these operations. Read more about path variables in  [Routing](https://aqueduct.io/docs/http/routing/).

The named constructor of  `Operation`  tells us which HTTP method the operation method handles. The following named constructors exist:

-   `Operation.post()`
-   `Operation.get()`
-   `Operation.put()`
-   `Operation.delete()`

> ⚡ The canonical  `Operation()`  constructor takes the HTTP method as its first argument for **non-standard** operations, e.g.:
```dart
@Operation('PATCH', 'id')
Future<Response> patchObjectWithID() async => ...;
```
All  `Operation`  constructors take a variable **list** of **path variables**. There can be multiple path variables for an operation. An operation method will **only be invoked** if **all of its path variables** are present in the request path. There can be multiple operation methods for a given HTTP method, as long as each expects a different set of path variables.

**Here's an example of an operation that requires two path variables:**
```dart
@Operation.get('userID', 'itemID')
Future<Response> getUserItem() async {
  final userID = request.path.variables['userID'];
  final itemID = request.path.variables['itemID'];
  return new Response.ok(...);
}
```
If no operation method exists for a request, a **405** **Method Not Allowed response** is automatically sent and no operation method is invoked.

### Routing to a ResourceController

A  `ResourceController`  subclass must be preceded by a  `Router`  in the application channel. The  `Router`  will parse path variables so that the controller can use them to determine which operation method should be invoked. A typical route to a  `ResourceController`  contains an optional identifying path variable:
```dart
router
  .route("/cities/[:name]")
  .link(() => new CityController());
```
This route would allow  `CityController`  to implement operation methods for all HTTP methods with both no path variables and the 'name' path variable.

It is considered good practice to break sub-resources into their own controller. For example, the following is preferred:
```dart
router
  .route("/cities/[:name]")
  .link(() => new CityController());

router
  .route("/cities/:name/attractions/[:id]")
  .link(() => new CityAttractionController());
```
By contrast, the route  `/cities/[:name/[attractions/[:id]]]`, while valid, makes controller logic much more unwieldy.

### Request Bindings

Operation methods may  _bind_  properties of an HTTP request to its parameters. When the operation method is invoked, the **value of that property** is passed as an **argument to the operation method**. 

> ⚡ For example, the following binds the header named **'X-API-Key'** to the argument  **`apiKey`**:
```dart
@Operation.get('name')
Future<Response> getCityByName(@Bind.header('x-api-key') String apiKey) async {
  if (!isValid(apiKey)) {
    return new Response.unauthorized();
  }

  return new Response.ok(...);
}
```

> ⚡ The following table shows the possible types of bindings:
>  

| property            | binding                         |
|---------------------|---------------------------------|
| Path Variable       | @Bind.path(pathVariableName)    |
| URL Query Parameter | @Bind.query(queryParameterName) |
| Header              | @Bind.header(headerName)        |
| Request Body        | @Bind.body()                    |

 -----------------------------------------

You may bind any number of HTTP request properties to a single operation method.


#### Optional Bindings

> ⚡ Bindings can be made optional. If a binding is optional, the operation method will still be called even if the bound property isn't in a request. To make a binding optional, move it to the optional parameters of an operation method:
```dart
@Operation.get()
Future<Response> getAllCities({@Bind.header('x-api-key') String apiKey}) async {
  if (apiKey == null) {
    // No X-API-Key in request
    ...
  }
  ...
}
```
A bound parameter will be null if not in the request. Like any other Dart optional parameter, you can provide a default value:
```dart
@Operation.get()
Future<Response> getAllCities({@Bind.header("x-api-key") String apiKey: "public"}) async {
  ...
}
```
#### Automatically Parsing Bindings

Query, header and path bindings can automatically be parsed into other types, such as  `int`  or  `DateTime`. Simply declare the bound parameter's type to the desired type:
```dart
Future<Response> getCityByID(@Bind.query('id') int cityID)
```
The type of a bound parameter may be  `String`  or any type that implements  `parse`  (e.g.,  `int`,  `DateTime`). Query parameters may also be bound to  `bool`  parameters; a boolean query parameter will be true if the query parameter has no value (e.g.  `/path?boolean`).

If parsing fails for any reason, an error response is sent and the operation method is not called. For example, the above example binds  `int cityID`  - if the path variable 'id' can't be parsed into an  `int`, a 404 Not Found response is sent. If a query parameter or header value cannot be parsed, a 400 Bad Request response is sent.

You may also bind  `List<T>`  parameters to headers and query parameters, where  `T`  must meet the same criteria as above. Query parameters and headers may appear more than once in a request. For example, the value of  `ids`  is  `[1, 2]`  if the request URL ends with  `/path?id=1&id=2`  and the operation method looks like this:
```dart
Future<Response> getCitiesByIDs(@Bind.query('id') List<int> ids)
```
Note that if a parameter is  _not_  bound to a list and there are multiple occurrences of that property in the request, a 400 Bad Request response will be sent. If you want to allow multiple values, you must bind to a  `List<T>`.

#### Header Bindings

The following operation method binds the header named  `X-API-Key`  to the  `apiKey`  parameter:
```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities(@Bind.header('x-api-key') String apiKey) async {
    if (!isValid(apiKey)) {
      return new Response.unauthorized();
    }

    return new Response.ok(['Atlanta', 'Madison', 'Mountain View']);
  }
}
```
If an  `X-API-Key`  header is present in the request, its value will be available in  `apiKey`. If it is not,  `getAllCities(apiKey)`  would not be called and a 400 Bad Request response will be sent. If  `apiKey`  were optional, the method is called as normal and  `apiKey`  is null or a default value.

Header names are case-insensitive per the HTTP specification. Therefore, the header name may be 'X-API-KEY', 'X-Api-Key' 'x-api-key', etc. and  `apiKey`  will be bound in all cases.

#### Query Parameter Bindings

The following operation methods binds the query parameter named 'name' to the parameter  `cityName`:
```dart
class CityController extends ResourceController {
  @Operation.get()
  Future<Response> getAllCities(@Bind.query('name') String cityName) async {
    return new Response.ok(cities.where((c) => c.name == cityName).toList());
  }
}
```
Query parameters can be required or optional. If required, a 400 Bad Request response is sent and no operation method is called if the query parameter is not present in the request URL. If optional, the bound variable is null or a default value.

Query parameters are case-sensitive; this binding will only match the query parameter 'name', but not 'Name' or 'NAME'.

Query parameters may also bound for query strings in the request body when the content-type is 'application/x-www-form-urlencoded'.

#### Path Variable Bindings

The following operation method binds the path variable 'id' to the parameter  `cityID`:
```dart
class CityController extends ResourceController {
  @Operation.get('id')
  Future<Response> getCityByID(@Bind.path('id') String cityID) async {
    return new Response.ok(cities.where((c) => c.id == cityID).toList());
  }
}
```
Path variables are made available when creating  [routes](https://aqueduct.io/docs/http/routing/). A  `Router`  must have a route that includes a path variable and that path variable must be listed in the  `Operation`  annotation. Path variables are case-sensitive and may not be optional.

If you attempt to bind a path variable that is not present in the  `Operation`, you will get a runtime exception at startup. You do not have to bind path variables for an operation method to be invoked.

#### HTTP Request Body Bindings

The body of an HTTP request can also be bound to a parameter:
```dart
class CityController extends ResourceController {
  CityController(this.context);

  final ManagedContext context;

  @Operation.post()
  Future<Response> addCity(@Bind.body() City city) async {
    final insertedCity = await context.insertObject(city);

    return new Response.ok(insertedCity);
  }
}
```
Since there is only one request body,  `Bind.body()`  doesn't take any identifying arguments.

> ⚡ The bound parameter type (**`City`**  in this example) must implement  **`Serializable`**. This interface requires two methods to be implemented: 
>  - **read data from a request body** 
>  - **write data to a response body**. 
>  Here is an example : 
```dart
class City extends Serializable {
  int id;
  String name;

  @override
  void readFromMap(Map<String, dynamic> map) {
    id = map['id'];
    name = map['name'];
  }

  @override
  Map<String, dynamic> asMap() {
    return {
      'id': id,
      'name': name
    }
  }
}
```


> 🔥 **ManagedObject and Serializable**
> `ManagedObject`s from Aqueduct's ORM implement  `Serializable`  without having to implement these two methods.

Aqueduct will automatically decode the request body from it's content-type, create a new instance of the bound parameter type, and invoke its  `readFromMap`  method. In the above example, a valid request body would be the following JSON:
```json
{
  "id": 1,
  "name": "Atlanta"
}
```
HTTP Body Decoding

Request bodies are decoded according to their content-type prior to being deserialized. For more information on request body decoding, including decoding content-types other than JSON, see  [this guide](https://aqueduct.io/docs/http/request_and_response/).

If parsing fails or  `readFromMap`  throws an exception, a 400 Bad Request response will be sent and the operation method won't be called.

You may also bind  `List<Serializable>`  parameters to the request body. Consider the following JSON that contains a list of cities:
```json
[
  {"id": 1, "name": "Atlanta"},
  {"id": 2, "name": "Madison"}
]
```
This body can be bound by declaring the bound parameter to be a  `List`  of the desired type:

Future<Response> addCity(@Bind.body() List<City> cities)

List vs Object

An endpoint should either take a single object or a list of objects, but not both. If the request body is a JSON list and the bound variable is not a list, a 400 Bad Request response will be sent (and vice versa). Declaring a body binding of the appropriate type validates the expected value and aids in automatically generating an OpenAPI specification for your application.

Note that if the request's  `Content-Type`  is 'x-www-form-urlencoded', its must be bound with  `Bind.query`  and not  `Bind.body`.

#### 🔥 Property Binding

The properties of an  `ResourceController`s may also have  `Bind.query`  and  `Bind.header`metadata. This binds values from the request to the  `ResourceController`  instance itself, making them accessible from  _all_  operation methods.
```dart
class CityController extends ResourceController {
  @requiredBinding
  @Bind.header("x-timestamp")
  DateTime timestamp;

  @Bind.query("limit")
  int limit;

  @Operation.get()
  Future<Response> getCities() async {
      // can use both limit and timestamp
  }
}
```
In the above, both  `timestamp`  and  `limit`  are bound prior to  `getCities`  being invoked. By default, a bound property is optional. Adding an  `requiredBinding`  annotation changes a property to required. If required, any request without the required property fails with a 400 Bad Request status code and none of the operation methods are invoked.


### Other ResourceController Behavior

Besides binding,  `ResourceController`s have some other behavior that is important to understand.

#### Request and Response Bodies

##### acceptedContentTypes

> ⚡ An  `ResourceController`  **can limit the content type of HTTP request bodies it accepts.** By default, an  `ResourceController`  will accept only  **`application/json`**  request bodies for its  **`POST`**  and  **`PUT`**  methods. This can be modified by setting the  **`acceptedContentTypes`**  property in the constructor.
```dart
class UserController extends ResourceController {
  UserController() {
    acceptedContentTypes = [ContentType.JSON, ContentType.XML];
  }
}
```
> ⚡ If a request is made with a content type other than the accepted content types, the controller automatically responds with a **415 Unsupported Media Type response.**

The body of an HTTP request is decoded if the content type is accepted and there exists a operation method to handle the request. The body is not decoded if there is not a matching operation method for the request. The body is decoded by  `ResourceController`  prior to your operation method being invoked. Therefore, 

> ⚡ you can always use the synchronous  **`RequestBody.as`**  method to access the body from within an operation method:
```dart
@Operation.post()
Future<Response> createThing() async {
  // do this:
  Map<String, dynamic> bodyMap = request.body.as();

  // no need to do this:
  Map<String, dynamic> bodyMap = await request.body.decode();

  return ...;
}
```
An  **`ResourceController`**  can also have a default content type for its responses. By default, this is  **`application/json`**. This default can be changed by changing  **`responseContentType`**  in the constructor:
```dart
class UserController extends ResourceController {
  UserController() {
    responseContentType = ContentType.XML;
  }
}
```
##### responseContentType

> ⚡ The  **`responseContentType`**  is the  _default_  response content type. An individual  `Response`  may set its own  `contentType`, which takes precedence over the  `responseContentType`. For example, the following controller returns **JSON** by default, but if the request specifically asks for XML, that's what it will return:
```dart
class UserController extends ResourceController {
  UserController() {
    responseContentType = ContentType.JSON;
  }

  @Operation.get('id')
  Future<Response> getUserByID(@Bind.path('id') int id) async {
    var response = new Response.ok(...);

    if (request.headers.value(Bind.headers.ACCEPT).startsWith("application/xml")) {
      response.contentType = ContentType.XML;
    }

    return response;
  }
}
```
#### More Specialized ResourceControllers

Many  `ResourceController`  subclasses will execute  [queries](https://aqueduct.io/docs/db/executing_queries/). There are helpful  `ResourceController`  subclasses for reducing boilerplate code.

##### QueryController<T>

> ⚡ A  `QueryController<T>`  builds a  **`Query<T>`**  based on the **incoming request.** 
> - If the request has a **body**, this  `Query<T>`'s  **`values`**  property is read from that body. 
>       --> where((u) => u.id).equalTo(id)
> - If the request has a **path variable**, the  `Query<T>`  assigns an expression to the **primary key value**. 
>       --> ..values = user;

> ⚡ ### highly recommended not to use queryController
>queryController 只能用在primaryKey與path一樣的條件下，不建議使用， 會有一些隱徵的問題出現，如path不是primary key 但該column有設indexed: true

For example, in a normal  `ResourceController`  that responds to a PUT request, you might write the following:
```dart
@Operation.put('id')
Future<Response> updateUser(@Bind.path('id') int id, @Bind.body() User user) async {
  var query = new Query<User>(context)
    ..where((u) => u.id).equalTo(id)
    ..values = user;

  return new Response.ok(await query.updateOne());
}
```
A  `QueryController<T>`  builds this query before a operation method is invoked, storing it in the inherited  `query`  property. A  `ManagedObject<T>`  subclass is the type argument to  `QueryController<T>`.
```dart
class UserController extends QueryController<User> {
  UserController(ManagedContext context) : super(context);

  @Operation.put('id')
  Future<Response> updateUser(@Bind.path('id') int id) async {
    // query already exists and is identical to the snippet above
    var result = await query.updateOne();
    return new Response.ok(result);
  }
}
```



A  `ManagedObjectController<T>`  can also be subclassed. 

> ⚡ A subclass allows for callbacks to be overridden to **adjust** the query **before execution**, or the results **before sending** the respond. 
> ⚡ Each operation - fetch, update, delete, etc. - has a pair of methods to do this. For example, the following subclass alters the query and results before any update via  `PUT`:

```dart
class UserController extends ManagedObjectController<User> {
  UserController(ManagedContext context) : super(context);

  Future<Query<User>> willUpdateObjectWithQuery(
      Query<User> query) async {
    query.values.lastUpdatedAt = new DateTime.now().toUtc();
    return query;
  }

  Future<Response> didUpdateObject(User object) async {
    object.removePropertyFromBackingMap("private");
    return new Response.ok(object);
  }
}
```
See the chapter on  [validations](https://aqueduct.io/docs/db/validations/), which are powerful when combined with  `ManagedObjectController<T>`.














<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNDE3Njg5MTUsMTkwMDIxMzQ4MCwxMj
A1OTQwMjI3LC01NDE5Njg3MDEsMTMzMzIzNTA1NiwtNjE0NDE5
NzE1LC0xMjU5NDAwMjc1LDIwMzM3MDM0OCw1MDc3Njg3NzAsLT
U2NzQzOTE3Niw0MTMwMzE0MTFdfQ==
-->

## Serving Files:


## Serving Files and Caching

Aqueduct can serve files by returning the contents of a file as an HTTP response body.

### FileController

> ✒️ Instances of  `FileController`  **serve a directory** from the filesystem through an HTTP interface. **Any route that channels requests to an  `FileController`**  _must_contain a  `*`  match-all token.

```dart
@override
Controller get entryPoint {
  final router = new Router();
  router.route("/files/*").link(() => new FileController("public/"));
  return router;
}
```
> ✒️ The argument to  `FileController`  is the **directory on the filesystem** in which request paths will be resolved against. In the above example, an HTTP request with the path  `/files/image.jpg`  would return the contents of the file  `public/image.jpg`.

> ⚡ Note that  **`public/`**  **does not** have a **leading slash** - therefore, the directory  `public`must be relative to the directory that the Aqueduct application was served from. 

In practice, this means you might have a directory structure like:
```
project/
  pubspec.yaml  
  lib/
    channel.dart
    ...
  test/
    ...
  public/
    image.jpg
```
Adding a **leading slash** to the directory served by  `FileController`  will resolve it relative to the **filesystem root**.

If the requested path was a directory, the filename  `index.html`  will be appended to the path when searching for a file to return.

If a file does not exist, an  `FileController`  returns a 404 Not Found response.

#### Content-Type of Files
##### setContentTypeForExtension - add content-types for extensions

An  `FileController`  will set the **content-type of the HTTP response** based on the served **files path** extension. By default, it recognizes many common extensions like  `.html`,  `.css`,  `.jpg`,  `.js`. You may add content-types for extensions to an instance:

```dart
var controller = new FileController("public/")
  ..setContentTypeForExtension("xml", new ContentType("application", "xml"));
```

> ✒️ If there is no entry for an extension of a file being served, the content-type defaults to  
`application/octet-stream`. An  `FileController`  will **never** invoke any **encoders** from  `CodecRegistry`, but it will **GZIP** data if the repository allows compression for the content-type of the file (see  `CodecRegistry.add`  and  `CodecRegistry.setAllowsCompression`).


### Caching
 
An  `FileController`  always sets the the Last-Modified header of the response to the last modified date according to the filesystem. If a request sends an If-Modified-Since header and the file has not been modified since that date, a 304 Not Modified response is sent with the appropriate headers.

You may provide Cache-Control headers depending on the path of the file being served. Here's an example that adds  `Cache-Control: public, max-age=31536000`

```dart
var policy = new CachePolicy(expirationFromNow: new Duration(days: 365));
var controller = new FileController("public/")
  ..addCachePolicy(policy, (path) => path.endsWith(".css"));
```

### File Serving and Caching Outside of FileController

A file can be served by any controller by setting the body object of a  `Response`  with its contents:
```dart
var file = new File("index.html");

// By loading contents into memory first...
var response = new Response.ok(file.readAsStringSync())
  ..contentType = new ContentType("application", "html");

// Or by streaming the contents from disk
var response = new Response.ok(file.openRead())
  ..encodeBody = false
  ..contentType = new ContentType("application", "html");
```

It is important to understand the how Aqueduct  [uses content-types to manipulate response bodies](https://aqueduct.io/docs/http/request_and_response/)  to serve file contents.

You may set the  `CachePolicy`  of any  `Response`. Note that  `CachePolicy`  only modifies the Cache-Control header of a response - headers like Last-Modified and ETag are not added.
```dart
var response = new Response.ok("contents")
  ..cachePolicy = new CachePolicy();
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NTkzNTUwNjYsMTI3NjAyMDUxNiwxNz
Q2NjAwNjYzLDE5NzkzMDU0MDZdfQ==
-->

## Validation:
 ## Validating Data

The preferred way of setting a validation is to add  `Validate`  metadata to properties of a table definition. Here's an example of a validation that ensures a tweet is less than 140 characters:
```dart
class Tweet extends ManagedObject<_Tweet> implements _Tweet {}
class _Tweet {
  @primaryKey
  int id;

  @Validate.length(lessThan: 140)
  String message;
}
```
#### Built-in Validators

There are a handful of built-in validations for common operations. For example, it is common to apply a regular expression to a value to ensure it formatted correctly or restrict the possible values to a list of available options. Common validators are available as named constructors of the  `Validate`  class. Here is an example:
```dart
class _Story {
  @primaryKey
  int id;

  @Validate.oneOf(const ["started", "accepted", "rejected", "delivered"])
  String state;
}
```
> ⚡ A built-in validator is useful because it automatically generates an error message that is returned in an HTTP response. For example, the previous code snippet indicates that the  `state`  property must be one of the four listed strings. If a value other than one of those four strings is used, the error message returned to the HTTP client would be:

**"The value `invalidValue` is not valid for `state`. Valid values are: 'started', 'accepted', 'rejected', 'delivered'.".**

See the API reference for  `Validate`  and its named constructors for possible options.
 `Validate`  annotations on transient properties have no effect. This annotation is only valid for properties declared in a table definition.


#### ⚡ Custom Validators

There will be times where the built-in validators are not sufficient for your application's use case. You may create subclasses of  `Validate`  to provide custom validation behavior. For example, if there were a  **`ValidatePhoneNumber`**  class:
```dart
class _Person {
  @primaryKey
  int id;

  @ValidatePhoneNumber()
  String phoneNumber;
}
```
A subclass of  `Validate`  must override  `Validate.validate()`  and call the superclass' primary constructor when instantiated. Here's an example of that phone number validator:
```dart
class ValidatePhoneNumber extends Validate {
  ValidatePhoneNumber({bool onUpdate: true, bool onInsert: true}) :
    super(onUpdate: onUpdate, onInsert: onInsert);

  @override
  void validate(ValidationContext context, dynamic value) {  
    if (value.length != 15) {
      context.addError("must be 15 digits");      
    }

    if (containsNonNumericValues(value)) {
      context.addError("must contain characters 0-9 only.");      
    }
  }
}
```

If  `value`  is doesn't meet the validation criteria, this method adds an error string to the  `ValidationContext`  it is passed. Error messages should be brief and indicate the successful criteria that failed. 

> ⚡ **Information about the property being validated will automatically** be added to the error message, so you **do not need** to include that information. If the context **has no errors** at the end of validation, the validation succeeds; otherwise, it fails.

A  `ValidationContext`  also has information about the property being validated, and whether the validation is running for an object being inserted or an object being updated.




#### Validation Behavior

A property may have more than one  `Validate`  metadata. In this case, all of the validations for a property must pass. The order in which multiple validations are performed is undefined and subject to change. Here's an example of validations that ensure a property's value is 10 characters long and only contains 10 alphabetic capital letters:
```dart
// property validator
@Validate.length(equalTo: 10)
@Validate.matches(r"$[A-Z]+^")
String tenCapitalLetters;
```

> ⚡ By default, validations are executed when a  `Query<T>`'s  **`insert`**  or  **`update`**  method is invoked. A validator can be restricted to only run on  **`insert`**  or  **`update`**  by passing values for its optional constructor arguments  **`onUpdate`**  and  **`onInsert`**:
```dart
// property validator
@Validate.matches(r"^[A-Z]+$", onInsert: true, onUpdate: false)
String validateOnInsertOnly;
```

It is important to understand how validations work when a value for a property is  **_not_**  specified in an insert or update query. For example, consider a  `Person`  with a  `name`  and  `email`  property and then inserted in a query where  `email`  hasn't been set:
```dart
var query = new Query<Person>(context)
  ..values.name = "Bob";

await query.insert();
```
Because  `email`  was not set on  `Query.values`, validations will not be run on that property.


**to require a property to be set: ** 
```dart
@Validate.present(onUpdate: false, onInsert: true)
String email;
```
> ⚡ The inverse of  `Validate.present()`  is  `Validate.absent()`. This validation **prevents a property from being set**. This is useful when a value should be included **during insertion**, but **can't be updated**. Here's an example:
```dart
@Validate.absent(onUpdate: true, onInsert: false)
String canOnlyBeSetOnce;
```

In the above declaration, the validator is only run on update operations and ensures that the property  `canOnlyBeSetOnce`  does not have a value. Because this validator is not run on insert operations, there is no restriction when the object is first inserted.

Validators are not run when a value is null. For example, the following insertion explicitly inserts  `null`  for the property  `email`:
```dart
var query = new Query<Person>(context)
  ..values.email = null
  ..values.name = "Bob";

await query.insert();
```
Nullability is enforced by  `Column.isNullable`  property. Consider the following declaration:
```dart
@Column(nullable: false)
@Validate.length(greaterThan: 10)
String name;
```


#### Other Validator Behavior

> ⚡ For validators that can't be built by subclassing  `Validate`, you may override  **`ManagedObject<T>.validate()`.** 
>  ⚡ This method is useful when a validation involves **more than one property**. Here's an example:
```dart
class Person extends ManagedObject<_Person> implements _Person {
  @override
  ValidationContext validate({Validating forEvent: Validating.insert}) {
   final ctx = super.validate(forEvent: forEvent);

    if (a + b > 10) {
      ctx.addError("a + b must be greater than 10");
    }

    return ctx;
  }
}
```
When overriding this method, the  `super`  implementation must be invoked to run validations managed by annotations. You must return the  `ValidationContext`  created by the superclass' implementation.

#### Skipping Validations

Validations are only run when values are set via  `Query<T>.values`. Values set via  `Query<T>.valueMap`  are not validated and is useful for inserting data without validation. Here's an example of skipping validation:
```dart
var query = new Query<Person>(context)
  ..valueMap = {
    "name" : "xyz",
    "email" : "whatever"
  };
```
#### Update and Insert Callbacks

> ⚡ `ManagedObject<T>`  subclasses may override  **`willUpdate`**  and  **`willInsert`**  to make changes prior to being updated or inserted. For example, a managed object may have updated and created dates that can be guaranteed to be set when inserted or updated:

```dart
class Person extends ManagedObject<_Person> implements _Person {
  @override
  void willUpdate() {
    updatedAt = new DateTime.now().toUtc();
  }

  @override
  void willInsert() {
    createdAt = new DateTime.now().toUtc();
  }
}
class _Person {
  @primaryKey
  int id;

  String name;
  DateTime createdAt;
  DateTime updatedAt;
}
```

Both  **`willUpdate`**  and  **`willInsert`**  are run **before any validation** occurs. Like validations,  `willUpdate`  and  `willInsert`  are skipped when using  `Query.valueMap`.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3MjU2NjE3NSwtMTEwNzU2NTQyNiw3OT
QyNzE4MjEsLTc4NzU4NDMzNCwxMDMxMzI2MjBdfQ==
-->

# Flutter
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [addActivityResultListener](#addactivityresultlistener)
- [Flutter/Android 專案目錄結構說明](#flutterandroid-%E5%B0%88%E6%A1%88%E7%9B%AE%E9%8C%84%E7%B5%90%E6%A7%8B%E8%AA%AA%E6%98%8E)
  - [說明Android Project wrapper for flutter](#%E8%AA%AA%E6%98%8Eandroid-project-wrapper-for-flutter)
  - [未完成](#%E6%9C%AA%E5%AE%8C%E6%88%90)
- [addNewIntentListener](#addnewintentlistener)
  - [Sample Code](#sample-code)
- [To Elaborate](#to-elaborate)
  - [BroadCastReceiver Overview](#broadcastreceiver-overview)
  - [BroadCastReceiver examples](#broadcastreceiver-examples)
  - [LocalBroadCastManager](#localbroadcastmanager)
  - [addNewIntentListener](#addnewintentlistener-1)
- [Channel android to flutter](#channel-android-to-flutter)
  - [Hiddable SliverAppbar](#hiddable-sliverappbar)
  - [TabBar Nested in SliverAppBar](#tabbar-nested-in-sliverappbar)
  - [Handy Tool for Preventing Rebuild](#handy-tool-for-preventing-rebuild)
- [Trouble Shooting](#trouble-shooting)
  - [Flutter Constaantly Rebuild](#flutter-constaantly-rebuild)
  - [Rendering Problems](#rendering-problems)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## addActivityResultListener

```mermaid
graph TB
	subgraph Native FlutterActivity
		pluginRegister
		pluginRegister -.- onCreate
		subgraph lifeCycles
			onCreate -.- onIntent
			onIntent -.- onResult
			onResult -.- onPause
			onResume -.- onPause
			onCreate -.- onStop
		end
	end
	subgraph Mediators-Flutter-UI-Plugin
		subgraph M1
			M1intent --- M1target
			M1intent --- M1data
			M1data --- M1filter
			M1filter --- M1target
		end
		MStartActivity
		MStartResult
		MStartActivity -.- MStartResult
		MStartResult -.- M1intent
	end
	
	subgraph FakeActivity
		FonCreate
		FonPause 
		FonResume 
		FonResult 
		FonIntent 
		FonReceive 
		
		subgraph fake activity
			FonPause -.- FonResume
			FonResume -.- FonCreate
			FonCreate --> Fcontainer
		end
		
		subgraph listeners
			Fcontainer --> FonIntent
			Fcontainer --> FonResult
			Fcontainer --> FonReceive
		end
		
		
		MStartActivity --> FonCreate
		MStartResult --> FonResult
	end
```

```mermaid
sequenceDiagram
participant Native
participant Mediator
participant Fake
participant Core


Note over Native, Fake: Scenario1: Before Ready

Native -->> Mediator: A1 initialize plugin
Mediator -->> Fake: A2 initialize fake activity
Fake -->> Fake: prepare intent logic
Fake -->> Mediator: A3 setup handlers to mediator
Mediator -->> Mediator: prepare dart channel
Mediator -->> Native: A4 Ready, set initial states
Native -->> Native: A5 setup events can't be delegate
Native -->> Mediator: A6 onPause, onResume...
Native -->> Mediator: hands over lifecycle control flow


Note over Mediator, Core: Scenario2: Ready - call core while discovering tag 

Mediator-->> Mediator: B0 discover tag, onNewIntent, findout receiver
Mediator-->>Mediator: B1 startActivity
Mediator-->>Core: B2 do work directly without another activity
Core-->>Mediator: B3 response
Mediator-->>Mediator: B4 Flutter UI


Note over Mediator, Core: Scenario3: Ready - startActivity while discovering tag

Mediator-->> Mediator: C0 discover tag, onNewIntent, findout receiver
Mediator-->>Fake: C1 onCreate
Fake-->>Fake: C2 do work
Fake-->>Core:C3 call core
Core-->>Fake:C4 response
Fake-->>Fake:C5 prepare intent
Fake->>Mediator: C6 startActivity from Fake
Mediator-->>Mediator: C7 startActivity->onNewIntent ...
Mediator-->>Fake: C8 onCreate


Note over Mediator, Core: Scenario4: Ready - startActivity triggered by Fake layer

Fake->>Mediator: D0 startActivity by user or...


Note over Mediator: Jump to Scenario 2
Note over Mediator: Jump to Scenario 3

```



## Flutter/Android 專案目錄結構說明

file structure of a flutter project would be look like this
- 📁 FlutterProject
	- 📰 pubspec.yaml 
	- 📁 lib [1]
- 	- 📁 res [2]
	- 📁 android [3]
		- 📁 app
			- 📁 libs
			- 📁 src
			- 📰 build.gradle 
		- 📁 res
		- 📁 assets
		- 📰 build.gradle
		- 📰 settings.gradle
		- 📰 local.properties 

>[1]  libraries written in dart.
>[2] android resource folder for flutter project,  generated by flutter.
>[3]  an android project wrapper for flutter, which contains following structure
>- 📁 app
>     - 📰 build.gradle
>     - 📰 local.properties 
>- 📁 res
>- 📁 assets
>- 📰 build.gradle
>- 📰 settings.gradle
> 


### 說明Android Project wrapper for flutter
#### file structure
- **FlutterProject > android**
	- 📁 app[^1]
		- 📁 libs
		- 📁 src
		- 📰 build.gradle [^2]
	- 📁 res[^*]
	- 📁 assets[^*]
	- 📰 build.gradle[^3]
	- 📰 settings.gradle[^4]
	- 📰 local.properties [^5]


**[1] app** your existing android project 
 #### [1] app folder - 現有android專案

 >需將其folder改名為app，app似乎為flutter默認的android專案名稱，以告知flutter專案設定的位置(build.gradle, settings.gradle)，如果不用默認的名稱似乎也可以，但需要設定project.name及include name，後文會寫到。
>
>android project folder, this would be your existing android project renamed to "app".  "app" seems like a default naming convention for flutter to indicate project entry folder where configurations(build.gradle and local.properties) locates. For not using convention name, you need to setup project name and include name in build.gradle, which will refer later on.
 
------------------------------------------------
 
**[2] build.gradle** : build script for existing android project
#### android專案設定檔 
>若要將現有專案與flutter連結,需在該設定檔中寫入以下資訊
- 載入local.properties
- flutterRoot 
- flutterProject
- apply pluginId **&** apply from
- sourceSets
- applicationId
- dependencies

> #### local.properties
> indicates where flutter.sdk and android.sdk locate.
```groovy
	def localProperties = new Properties()  
	def localPropertiesFile = rootProject.file('local.properties')  
	if (localPropertiesFile.exists()) {  
	    localPropertiesFile.withReader('UTF-8') { reader ->  
	        localProperties.load(reader)  
	    }  
	}
```
> 主要記錄了flutter.sdk及android.sdk的位置, 介由讀取 local.properties 以注入flutter
> - rootProject 指向 android , 
> - localPropertiesFile  指向 android/local.properties

> #### flutterRoot
> read flutter sdk from local.properties into flutterRoot
```groovy
	def flutterVersionCode = localProperties.getProperty('flutter.versionCode')  
	if (flutterVersionCode == null) {  
	    flutterVersionCode = '1'  
	}  
	def flutterVersionName = localProperties.getProperty('flutter.versionName')  
	if (flutterVersionName == null) {  
	    flutterVersionName = '1.0'  
	}
	def flutterRoot = localProperties.getProperty('flutter.sdk')  
	if (flutterRoot == null) {  
	    throw new GradleException("Flutter SDK not found. Define location with flutter.sdk in the local.properties file.")  
	}
```

> #### flutterProject
> specify where flutterProject locates, default value are "../.."
```groovy
	def flutterProject = '../..'
	flutter {  
	    source flutterProject  
	}
```

> #### apply plugin | apply from
> contains following two sections
>  - apply plugins like kotlin which indicates a valid pluginId implmented the plugin interface by project requirements.
>  - apply a build script from flutter
```groovy
	apply plugin: 'com.android.application'  
	apply plugin: 'kotlin-android'  
	apply from: "$flutterRoot/packages/flutter_tools/gradle/flutter.gradle"
```

> #### sourcesSet
> set entry point for finding sources
> - java source
> - res source
	> res 分為二個部份，一個是原有專案的res, 另一個則為flutter專案所需要的 res，由flutter自動產生
>   - res resource of existing android project
>   - res source of current flutter project	
> - assets source
```groovy
	android {  
		sourceSets {  
			main {  
				//manifest.srcFile "src/main/AndroidManifest.xml"  
				java.srcDirs += ['src/main/kotlin', 'src/main/java']  
				res.srcDirs += ['res', flutterProject + '/res']  
				assets.srcDirs += ['assets', flutterProject + '/assets']  
			}  
```

> #### applicationId
> 
```groovy
	android {
		defaultConfig {  
			// applicationId will replace package name specified in AndroidManifest.xml  
			applicationId "com.gknot"  
			minSdkVersion 19  
			targetSdkVersion 28  
			versionName flutterVersionName  
			testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"  
		}
```

> #### dependencies
```groovy
	dependencies {  
		implementation fileTree(include: '*.jar', dir: 'libs')  
		implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"  
		//following dependencies depends on flutter  
		testImplementation 'junit:junit:4.12'  
		androidTestImplementation 'com.android.support.test:runner:1.0.2'  
		androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'  
	}
```
[file](build.gradle.andoirdpj)

------------------------------------------------

**[*]res and assets** for existing android project
#### 設置android專案 res & assets
> 於上述build.gradle中設定

------------------------------------------------

**[3] build.gradle [4] setting.gradle** for flutter building android
#### flutter android build 專案設定檔
- **build.gradle**
	- kotlin & gradle dependencies
	- add buildDir
	- add existing android project into subproject
	- add clean build
- **settings.gradle**
	- include subproject
	- add flutter plugin
 
```groovy
	// -------------------
	// flutterProject/android/build.gradle....
	buildscript {  
		ext.kotlin_version = '1.3.21'  
		dependencies {  
			classpath 'com.android.tools.build:gradle:3.3.1'  
			classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"  
		}  
	}  
	rootProject.buildDir = '../build'  
	subprojects {  
		project.buildDir = "${rootProject.buildDir}/${project.name}"  
	}  
	subprojects {  
		project.evaluationDependsOn(':app')  
	}  
	task clean(type: Delete) {  
		delete rootProject.buildDir  
	}

	// -------------------
	// flutterProject/android/settings.gradle....
	include ':app'  
	def flutterProjectRoot = rootProject.projectDir.parentFile.toPath()  
	def plugins = new Properties()  
	def pluginsFile = new File(flutterProjectRoot.toFile(), '.flutter-plugins')  
	if (pluginsFile.exists()) {  
	    pluginsFile.withReader('UTF-8') { reader -> plugins.load(reader) }  
	}  
	plugins.each { name, path ->  
	    def pluginDirectory = flutterProjectRoot.resolve(path).resolve('android').toFile()  
	    include ":$name"  
	  project(":$name").projectDir = pluginDirectory  
	}
```
> **[NOTE]** the **colon** in Gradle uses to describe paths to subprojects. For example,
> evaluationDependsOn(':api:producer')
> would look for the subproject  `producer`  of the subproject  `api`.

------------------------------------------------

**[5] local.properties**
#### 連結android sdk, flutter sdk
```groovy
	sdk.dir=D:/Users/gordianknot/AppData/Local/Android/android-sdk  
	flutter.sdk=E:\\flutter  
	flutter.versionName=1.0.0  
	flutter.versionCode=1  
	flutter.buildMode=debug
```  

### 未完成
上述方法用了flutter i18n plugin 後由會在 res/value 底下加入 string_xx.arb ,導致編譯時出現 "file name must end with .xml", 目前找不到合適的解決方法，只好手動把flutter 的 resource copy 到 原 android 專案中，並把
```groovy
	android {  
		sourceSets {  
			main {  
				//manifest.srcFile "src/main/AndroidManifest.xml"  
				java.srcDirs += ['src/main/kotlin', 'src/main/java']  
				//res.srcDirs += ['res', flutterProject + '/res']  
				res.srcDirs += ['res']  
				assets.srcDirs += ['assets', flutterProject + '/assets']  
			}  
```
或者只好停用 android studio Flutter i18n 的 plugin



 
```mermaid
graph LR
	fdk((flutter sdk))
	adk((android sdk))
	
	dfb{buildDir}
	abuild((android build))
	fpgn((flutter plugin))
	style abuild stroke-width:6px   
	style fpgn stroke-width:6px 
	style dfb stroke-width:6px 
	
	loc(local.properties)
	style loc stroke-width:4px   
	
	set(setting.gradle)
	b1(android wrapper - build.gradle)
	b2(android project - build.gradle)
	style b1 stroke-width:4px, stroke:#7bc
	style b2 stroke-width:4px, stroke:#7bc
	style set stroke-width:4px, stroke:#7bc

	dfp(flutterProjectRoot-resolve project path)
	
	
	dandroid(dependencies-androidProject)
	dflutter(dependencies-buildFlutter)
	dsub(subproject-relationship)
	style dandroid stroke-width:4px, stroke-dasharray: 5
	style dflutter stroke-width:4px, stroke-dasharray: 5
	style dsub stroke-width:4px, stroke-dasharray: 5
	
	
subgraph one
	subgraph path resolving
		fdk -.-> loc
		adk -.-> loc
	end

	loc -.read path information .-> b2
	subgraph android project resolving
		b2 -.- abuild
	end
	dfp -.- b2
	subgraph projectPath&plugin resolving
		set-.->dfp
		set -.- fpgn
	end
	dandroid -.- b2
	subgraph resolving flutter build
		b1 -.- dfb
		
		dflutter -.- b1
		dsub -.- b1
		dflutter -.- dsub
		dsub-.- dandroid
	end
end
```
 
**Demo project**
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkxNjgwODAyNCwtMTgyMzA5NDAxNCwtMT
g3MjM3MzM3MCwxODE4NDY0NjYwLDE5NDA5NTM2MDIsMTcyNDE5
NzY0MSwxMDE4MzkxNDYyLDE0NjAzMDgwNTQsMTU2MTAyMjMyOS
w4MDEyNDMyNDYsLTM2NDY4MDMyMSwtMTkzNjQ3OTI1NSwtMTc3
NDY5Njg4Ml19
-->


## addNewIntentListener
### Sample Code
[fetched from github](https://github.com/flutter/plugins/blob/master/packages/firebase_messaging/android/src/main/java/io/flutter/plugins/firebasemessaging/FirebaseMessagingPlugin.java)
```java
import  android.content.BroadcastReceiver;
import io.flutter.plugin.common.MethodCall;
import io.flutter.plugin.common.MethodChannel;
import io.flutter.plugin.common.MethodChannel.MethodCallHandler;
import io.flutter.plugin.common.MethodChannel.Result;
import io.flutter.plugin.common.PluginRegistry.NewIntentListener;
import io.flutter.plugin.common.PluginRegistry.Registrar;

public class FirebaseMessagingPlugin extends BroadcastReceiver implements MethodCallHandler, NewIntentListener {
	private final Registrar registrar;
	private final MethodChannel channel;

	private static final String CLICK_ACTION_VALUE = "FLUTTER_NOTIFICATION_CLICK";
	private static final String TAG = "FirebaseMessagingPlugin";

	public static void registerWith(Registrar registrar) {
		final MethodChannel channel =
		new MethodChannel(registrar.messenger(), "plugins.flutter.io/firebase_messaging");
		final FirebaseMessagingPlugin plugin = new FirebaseMessagingPlugin(registrar, channel);
		registrar.addNewIntentListener(plugin);
		channel.setMethodCallHandler(plugin);
	}

	private FirebaseMessagingPlugin(Registrar registrar, MethodChannel channel) {
		this.registrar = registrar;
		this.channel = channel;
		FirebaseApp.initializeApp(registrar.context());
		IntentFilter intentFilter = new IntentFilter();
		intentFilter.addAction(FlutterFirebaseInstanceIDService.ACTION_TOKEN);
		intentFilter.addAction(FlutterFirebaseMessagingService.ACTION_REMOTE_MESSAGE);
		LocalBroadcastManager manager = LocalBroadcastManager.getInstance(registrar.context());
		manager.registerReceiver(this, intentFilter);
	}

	// BroadcastReceiver implementation.
	@Override
	public void onReceive(Context context, Intent intent) {
		String action = intent.getAction();
		if (action == null) {
			return;
		}
		String tok_service = FlutterFirebaseInstanceIDService.EXTRA_TOKEN;
		String rem_service = FlutterFirebaseInstanceIDService.ACTION_REMOTE_MESSAGE;
		if (action.equals(tok_service )) {
			String token = intent.getStringExtra(tok_service );
			channel.invokeMethod("onToken", token);
		} else if (action.equals(rem_service )) {
			RemoteMessage message = intent.getParcelableExtra(rem_service );
			Map<String, Object> content = parseRemoteMessage(message);
			channel.invokeMethod("onMessage", content);
		}
	}

	@NonNull
	private Map<String, Object> parseRemoteMessage(RemoteMessage message) {
		...
	}

	@Override
	public void onMethodCall(final MethodCall call, final Result result) {
		.......
	}

	@Override
	public boolean onNewIntent(Intent intent) {
		boolean res = sendMessageFromIntent("onResume", intent);
		if (res && registrar.activity() != null) {
			registrar.activity().setIntent(intent);
		}
		return res;
	}

	/** @return true if intent contained a message to send. */
	private boolean sendMessageFromIntent(String method, Intent intent) {
		if (CLICK_ACTION_VALUE.equals(intent.getAction())
			|| CLICK_ACTION_VALUE.equals(intent.getStringExtra("click_action"))) {
			Map<String, String> message = new HashMap<>();
			Bundle extras = intent.getExtras();

			if (extras == null) {
				return false;
			}

			for (String key : extras.keySet()) {
				Object extra = extras.get(key);
				if (extra != null) {
					message.put(key, extra.toString());
				}
			}

			channel.invokeMethod(method, message);
			return true;
		}
		return false;
	}
}
```

## To Elaborate
### BroadCastReceiver Overview
>[BroadcastReceiver](http://developer.android.com/reference/android/content/BroadcastReceiver.html) 是一個相當常用的類別，它可以用來監聽廣播，並做一些事情。  我們可以先創建一個類別，並且繼承BroadcastReceiver這個類別，實作它要我們做的方法。

```java
	public class MyBroadcastReceiver extends BroadcastReceiver {
		@Override
		public void onReceive(Context context, Intent intent) {
		}
	}
```
>當有人發送廣播給這個BroadcastReceiver時，就會觸發onReceive方法。   **別忘了在AndroidManifest.xml加入這個receiver。**  
```xml
	<receiver android:name=".MyBroadcastReceiver"></receiver>  
```
>下一步，你必須把這個receiver註冊到背景之中。  此時你有兩種選擇。  第一種，在程式片段裡面註冊。  這種的好處是，你可以在特定的頁面裡面在註冊(register)接收器，而且可以註銷(**unregister**)你的接收器。

```java
	//創建一個IntentFilte物件
	IntentFilter intentFilter = new IntentFilter();
	//加入Action的辨識字串
	intentFilter.addAction("Hello");
	//註冊我們創建的BroadcastReceiver
	registerReceiver(new MyBroadcastReceiver() , intentFilter);
	Intent intent = new Intent();
	intent.setAction("Hello");
	sendBroadcast(intent);
```
>以FlutterFireBasePlugin 為例 則使用了 **localBrocastManager**
```java
	IntentFilter intentFilter = new IntentFilter();
	intentFilter.addAction("path.to.action.name");
	LocalBroadcastManager manager = LocalBroadcastManager.getInstance(registrar.context());
	manager.registerReceiver(this, intentFilter);
```


----
### BroadCastReceiver examples

- BroadCastReceiver > register on resum, unregister on pause
  : Example on Network Connection Receiver

```xml
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```
```java
@Override  
public void onResume() { 
	super.onResume(); 
	// 註冊mConnReceiver，並用IntentFilter設置接收的事件類型為網路開關  	
	this.registerReceiver(
		mConnReceiver,
		new IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION)); 
}
@Override  
public void onPause() { 
	super.onPause(); 
	// 解除註冊  
	this.unregisterReceiver(mConnReceiver); 
}
```

- BroadCastReceiver > onReceive
	: Example on Network Connection Receiver
```java
	// BroadcastReceiver implementation.
	@Override
	public void onReceive(Context context, Intent intent) {
		// get action string from intent
		String action = intent.getAction();
		if (action == null) {
			return;
		}
		if (action.equals("action_name") {
			String token = intent.getStringExtra("action_name");
			channel.invokeMethod("onToken", token);
		} else if (action.equals("action_nameB") {
			RemoteMessage message =
			intent.getParcelableExtra("action_nameB");
			Map<String, Object> content = parseRemoteMessage(message);
			channel.invokeMethod("onMessage", content);
		}
	}
```



### LocalBroadCastManager

在寫Android應用時候，有時候或多或少的需要運用廣播來解決某些需求，我們知道廣播有一個特性，就是使用**sendBroadcast**(intent);發送廣播時，手機內所有注冊了**BroadcastReceiver**的應用都可以在接收到這個廣播的，並在**BroadcastReceiver**的**onReceive**()方法進行匹配，而應用是否使用這個廣播則是取決與我們定義的Action與廣播接收者的是否匹配，也就是說平常我們使用的廣播是全局的廣播，誰都有權收到。所以這就有可能產生安全漏洞和隱私數據泄密：

> - 假如別人反編譯你的apk後知道了你的Action，那麼第三方應用就可以發送與該Action匹配的廣播，而你的應用也可以接收到，所以這就有可能被第三方利用這個來搞一些事
> - 同樣假如別人知道了你應用內的Action，當你使用廣播來傳輸一些數據的時候，而其它應用也能接受到這個廣播，通過Action匹配，就有可能獲取到你的私密數據

- **LocalBroadcastManager**.getInstance
	- params 
		- **Context** context 
	- @return **LocalBroadcastManager**

- **LocalBroadcastManager**
	- registerReceiver  | unregisterReceiver
		- params
			- **BroadcastReceiver** receiver
			- **IntentFilter** filter
	- sendBroadCast  
		-  params
			- **Intent** intent

```java
	IntentFilter intentFilter =  new  IntentFilter();
	intentFilter.addAction("path.to.action.name"); 
	LocalBroadcastManager manager = LocalBroadcastManager.getInstance(context); 
	manager.registerReceiver(receiver, intentFilter);
```

#### LocalBroadcastManager cannot be used with pendingIntent
The point behind a  `PendingIntent`  is to allow  _some other process_to perform an action you request, such as sending a broadcast. The point behind  `LocalBroadcastManager`  is to keep broadcast  _within your process_. Hence, a  `PendingIntent`  can issue a regular broadcast, but not one via  `LocalBroadcastManager`.

`PendingIntent` cannot work with `LocalBroadcastManager`. A `PendingIntent` is designed to work across process boundaries; `LocalBroadcastManager` is designed to _not_ work across process boundaries.


### addNewIntentListener
**definition**
```java
Registrar.addNewIntentListener(NewIntentListener listener)
interface NewIntentListener {
	boolean onNewIntent(Intent intent)
}
```
**example**



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzMjczOTI5MCwtMTQ4NTU5ODQxNywyNj
c0MDY0NDYsLTcxOTk3NDk3Nyw4NDE4MzMzODBdfQ==
-->

 
## Channel android to flutter

```mermaid
sequenceDiagram
participant Main
participant Mediator
participant DartChannel
participant Nlib

Note over Main, Nlib: 👷 Scenario - Start App
Main -->> DartChannel: register plugins
Main -->> Mediator: register mediators
Main -->> Nlib: instantiate native library
Nlib -->> Mediator: add to Mediator

```







--------------------------------------------------------

**Mediator In Detail**
>**MOn**: Mediator Events  |  **MAct**: Mediator Actions  |  **FOn**: Events of FakeActivity 
>**FDo**: Logic of Fake Activity  |  **JDo**: Logic of Core Lib
```mermaid
sequenceDiagram
participant MUi
participant MEvent
participant MProc
participant MAct
participant .

MProc-->>MProc: initialize plugins ❓
Note over MProc,MAct: A1) instantiate all FakeActivity and strip useless code in onCreate<br>A2) setup intentFilters for each FakeActivity<br>📙 Note: brainstorming-A2

MProc-->>MProc: prepare intenFilter mappings ❓

Note over MProc, MAct: B1) each fakeActivity has several intentFilters<br>B2) each fakeActivity map to a unique string action<br>__represents an explicit activity request.<br>_<br> find out who is requesting from and to.
MProc-->> .: onCreate for initialization and intial detection
MProc-->>MEvent: Ready!



Note over MUi, .: 👷 Scenario1: startActivity while discovering Tag<br>.<br>.

Note over MEvent, .: 👷 Scenario1-A: onNewIntent [O]<br>. . . .call onNewIntent or not by comparing current fakeActivity with intent new one<br>. . . .if it's the same then call onNewIntent, if it's not then call onCreate<br>📙 Note: brainstorming-A1
MEvent-->>MEvent: onNewIntent [O]
MEvent-->>MProc: getTargetByIntent [V]
MProc-->>MProc: getTargetByIntent
MProc-->>MProc: startFakeActivityExplicit - already created
MProc-->>. : onNewIntent [O]
. -->> . : onNewIntent[O] process ❓

Note over MEvent, .: 👷 Scenario1-B: onNewIntent [X]<br>.
MEvent-->>MEvent: onNewIntent [X]
MEvent-->>MProc: getTargetByIntent [V]
MProc-->>MProc: getTargetByIntent
MProc-->>MProc: startFakeActivityExplicit - not created
MProc-->>MProc: createFakeActivity
MProc->> . : onCreate
. -->> . : process ❓
Note left of .: e.g. vitration, initiate Ntag, startDemo,<br>show auth dialog


Note over MProc, .: 👷 Scenario1-B1: delegate ui to user<br>.
. ->> MUi: UI response, listen for result [O] [X]
MUi -->> MEvent: listen for result  [O] [X]
MUi -->> MUi: user interact or none
MUi -->> MEvent: onResult
MEvent -->> MEvent: onResult
MProc -->> MEvent: getTargetByIntent(context, intent)
MEvent -->> . : onActivityResult


Note over MProc, .: 👷 Scenario1-B2: direct ui to user<br>.
. ->> MAct: startFakeActivity
MAct -->> MAct: startFakeActivity
MAct -->> MProc: getTargetByIntent
MProc -->> MProc: getTargetByIntent
MProc -->> MProc: startExplicitActivity
MProc -->> MProc: createFakeActivity
MProc ->> . : onCreate
. -->> MUi : Ui response



Note over MUi, .: 👷 Scenario2: startActivityForResult while trigger auth request<br>.<br>.

Note over MEvent, .: 👷 Scenario2-A: onNewIntent [O]<br>. . . . 
MEvent-->>MEvent: onNewIntent [O]
MEvent-->>. : onNewIntent [O]
. -->> . : onNewIntent[O] process ❓

MEvent-->>MEvent: onNewIntent
MEvent-->>MProc: getTargetByIntent [V]
MProc-->>MProc: getTargetByIntent
MProc-->>MProc: startFakeActivityForResultExplicit
MProc-->>MProc: createFakeActivityForResult
MProc->> . : onCreate - open auth
. -->> . : process ❓
Note left of .: e.g. show auth dialog
. ->> MUi: UI response, listen for result [O] [X]
MUi -->> MUi: user interact or none
MUi -->> MEvent: listen for result  [O] [X]
MEvent -->> MEvent: onActivityResult, result passed from dart to platform
MEvent ->> . : call onActivityResult with result
. -->> . : onActivityResult [pass] [failed]
. ->> MAct: startFakeActivity
MAct -->> MAct: startFakeActivity
MAct -->> MProc: getTargetByIntent
MProc -->> MProc: getTargetByIntent
MProc -->> MProc: startExplicitActivity
MProc -->> MProc: createFakeActivity
MProc ->> . : onCreate  

```
**brainstorming in A1 -  how onNewIntent in behavior - IDEATION**
- fakeActivity has no ui resources
	- once instantiated, never mind disposing for release resources just like normal library
		- fakeActivity only create once - onCreate only called once
			- ❓ allways call onNewIntent while receiving intent requests???
	- in real Activity: each Activity might hold its own resources, but fakeActivity should not operate like that
		- some resources can be shared
			- current intent & nfcintent
				- difference between the two ❓
			- demo library
			- 

**narrow down**:
- instantiate each fakeActivity without constructor
- dependencies thru injection
- DI thru mediator
	- ntag demo, intent, nfcadapter 
- remember nfcintent - tag detected
- call onNewIntent while already instantiated


**brainstorming in A2 - setup intentFilter for each FakeActivity - IDEATION**
- what we already have
	- intent matching logic; but it may not work while exposing to customized mediator
	- matching logic only works under some circumstances ---- applied after intentFilter written in manifest.xml
- prepare intentFilter for each FakeActivity while initializing each FakeActivity

**narrow down**
- implement intentFilter manually instread of using the one in manifest.xml




-------------------------------------------------------
**FakeActivity in Detail**

```mermaid
sequenceDiagram
participant .
participant FEvent
participant FAct
participant Core

Note over ., Core: Scenario1: Initialize All FakeActivities
. ->> FEvent :onCreate
FEvent -->> FEvent: onCreate, initial detection
FEvent -->> Core: Initialize NtagDemo
Core -->> .: share dependency

Note over ., Core: Scenario2: startActivity
. ->> FEvent: onNewIntent
FEvent -->> FEvent: onNewIntent
FEvent -->> FAct: doProcess
FAct -->> FAct: process
FAct -->> Core: ntag
Core -->> FAct: response
FAct -->> FAct: parse
FAct ->> . : UI response or start Activity, listen for Result [O] [X]


```



--------------------------------------------------------



```mermaid
sequenceDiagram
participant Activity
participant Mediator
participant Fake
participant Jar


Note over Activity, Fake: 👷 Scenario1: Before Ready

Activity -->> Mediator: A1 initialize plugin
Mediator -->> Fake: A2 initialize fake activity
Fake -->> Fake: prepare intent logic
Fake -->> Mediator: A3 setup handlers to mediator
Mediator -->> Mediator: prepare dart channel
Mediator -->> Activity: A4 Ready, set initial states
Activity -->> Activity: A5 delegate lifecycle
Activity -->> Mediator: Finish handing over lifecycle control flow to Mediator


Note over Mediator, Jar: 👷 Scenario2: Ready - call Jar while discovering tag 

Mediator-->> Mediator: B0 discover tag, onNewIntent, findout receiver
Mediator->>Jar: B1 do work directly without starting another activity
Jar->>Mediator: B2 response
Mediator-->>Mediator: B3 Flutter UI


Note over Mediator, Jar: 👷 Scenario3: Ready - startActivity while discovering tag

Mediator-->> Mediator: C0 discover tag, onNewIntent, findout receiver
Mediator-->>Mediator: C0 startActivity
Mediator->>Fake: C1 onCreate
Fake-->>Fake: C2 do work
Fake->>Jar:C3 call Jar
Jar->>Fake:C4 response
Fake-->>Fake:C5 prepare intent
Fake->>Mediator: C6 startActivity from Fake
Mediator-->>Mediator: C7 startActivity->onNewIntent ...
Mediator->>Fake: C8 onCreate


Note over Mediator, Jar: 👷 Scenario4: Ready - startActivity triggered by Fake layer

Fake->>Mediator: D0 startActivity by user or...


Note over Mediator: Jump to Scenario 2
Note over Mediator: Jump to Scenario 3


Note over Mediator, Jar: 👷 Scenario5: Ready - startActivityForResult while discovering tag
Mediator-->> Mediator: E0 discover tag, onNewIntent, findout receiver
Mediator-->>Mediator:E1 startActivityForResult
Mediator-->>Fake: E2 onCreate
Fake-->>Fake: E3 do work
Fake-->>Jar:E4 call Jar
Jar-->>Fake:E5
Fake->>Mediator: onActivityResult
Mediator-->>Mediator: UI 
```

------------------
 









Original System
: **A1**: Action1, **A2**: Action2, **Sys**: internal system
```mermaid
sequenceDiagram
participant A1
participant A2
participant Sys

Note over A1, A2: Scenario1: Implicit intent
A1 -->> A1: prepare intent
A1 -->> A1: startActivity with explicit target
A1 ->>  A2: onCreate

Note over A1, Sys: Scenario2: Explicit intent
A1 -->> A1: prepare intent
A1 -->> Sys: startActivity Implicitly
Sys-->> Sys: filterIntent
Sys-->> Sys: startActivity with explicit target
Sys ->> A2: onCreate
``` 

--------------------------------------------------------

Replace Sys with Mediator
: **A1**: Action1, **A2**: Action2, **Mediator**: customized mediator for mimic-behaviors of internal system
```mermaid
sequenceDiagram
participant A1
participant A2
participant Mediator

Note over A1, A2: Scenario1: Implicit intent
A1 -->> A1: prepare intent
A1 -->> Mediator: startActivity with explicit target
Mediator -->> Mediator: prepare target
Mediator->>  A2: onCreate

Note over A1, Mediator: Scenario2: Explicit intent
A1 -->> A1: prepare intent
A1 -->> Mediator: startActivity Implicitly
Mediator-->> Mediator: prepare target
Mediator->> A2: onCreate
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQ5MDM4NDA0LC0yMTExMDU5MjMwLDQ3NT
Q4MTc4MCwtOTk2MzEyNDQwLDE5OTUxOTQ4NTcsMjAzNjA3OTI4
MiwtMTM2ODY0NDE2NiwtMTQ2MDE2MzgwNiwtMTcyMTk0MTkyMy
wxNTQ1NTc1NzQsNzI3NTE5NzgsMTg1MDE1ODkzNiwtMTE0MjYx
Mzk5MywtMzg4MzM2OTU3LDEzNzgyMTk3MDgsLTEyMTQ0OTA3Mz
UsLTMwODIzMDQ4NCw2MDM4MDM2NDgsLTE0NTM4ODAwMDgsLTE0
NzYyNjIzODNdfQ==
-->




### Hiddable SliverAppbar


### TabBar Nested in SliverAppBar
#### NestedScrollView / SliverAppBar / SliverPersistentHeader / 

```mermaid
graph LR
	NS[NestedScrollView] --> SA[SliverAppBar]
	NS[NestedScrollView] --> SPH[SliverPersistentHeader]
	SPH[SliverPersistentHeader] --> SAD[SliverAppBarDelegate]
	SAD[SliverAppBarDelegate] --> TAB[TabBar]
	TAB[TabBar] --> TBS1[tabs...]
	TAB[TabBar] --> TBSN[tabs...]
```

##### _SliverAppBarDelegate
```dart
  class _SliverAppBarDelegate extends SliverPersistentHeaderDelegate {
    _SliverAppBarDelegate(this._tabBar);

    final TabBar _tabBar;

    @override
    double get minExtent => _tabBar.preferredSize.height;
    @override
    double get maxExtent => _tabBar.preferredSize.height;

    @override
    Widget build(
        BuildContext context, double shrinkOffset, bool overlapsContent) {
      return new Container(
        child: _tabBar,
      );
    }

    @override
    bool shouldRebuild(_SliverAppBarDelegate oldDelegate) {
      return false;
    }
  }
```


##### NestedScrollView
```dart
Widget build(BuildContext context) {
      return Scaffold(
        body: DefaultTabController(
          length: 2,
          child: NestedScrollView(
            headerSliverBuilder: (BuildContext context, bool innerBoxIsScrolled) {
              return <Widget>[
                SliverAppBar(
                  expandedHeight: 200.0,
                  floating: false,
                  pinned: true,
                  flexibleSpace: FlexibleSpaceBar(
                      centerTitle: true,
                      title: Text("Collapsing Toolbar",
                          style: TextStyle(
                            color: Colors.white,
                            fontSize: 16.0,
                          )),
                      background: Image.network(
                        "https://images.pexels.com/photos/396547/pexels-photo-396547.jpeg?auto=compress&cs=tinysrgb&h=350",
                        fit: BoxFit.cover,
                      )),
                ),
                SliverPersistentHeader(
                  delegate: _SliverAppBarDelegate(
                    TabBar(
                      labelColor: Colors.black87,
                      unselectedLabelColor: Colors.grey,
                      tabs: [
                        Tab(icon: Icon(Icons.info), text: "Tab 1"),
                        Tab(icon: Icon(Icons.lightbulb_outline), text: "Tab 2"),
                      ],
                    ),
                  ),
                  pinned: true,
                ),
              ];
            },
            body: Center(
              child: Text("Sample text"),
            ),
          ),
        ),
      );
    }
```



###  Handy Tool for Preventing Rebuild

```dart 
class ShouldRebuild<T extends Widget> extends StatefulWidget {
  final T child;
  final ShouldRebuildFunction<T> shouldRebuild;
  ShouldRebuild({@required this.child, this.shouldRebuild}):assert((){
    if(child == null){
      throw FlutterError.fromParts(
          <DiagnosticsNode>[
            ErrorSummary('ShouldRebuild widget: builder must be not  null')]
      );
    }
    return true;
  }());
  @override
  _ShouldRebuildState createState() => _ShouldRebuildState<T>();
}

class _ShouldRebuildState<T extends Widget> extends State<ShouldRebuild> {
  @override
  ShouldRebuild<T> get widget => super.widget;
  T oldWidget;
  @override
  Widget build(BuildContext context) {
    final T newWidget = widget.child;
    if (this.oldWidget == null || (widget.shouldRebuild == null ? true : widget.shouldRebuild(oldWidget, newWidget))) {
      this.oldWidget = newWidget;
    }
    return oldWidget;
  }
} 
```

#### usage
```dart 
ShouldRebuild<Counter>(
	shouldRebuild: (oldWidget, newWidget) => oldWidget.counter != newWidget.counter,
	child: Counter(counter: counter,onClick: _incrementCounter,title: '我是优化过的Counter',) ,
),
```


#### 節錄flutter 判斷是否updateChild
```dart
@protected  
Element updateChild(Element child, Widget newWidget, dynamic newSlot) {  
  assert(() {  
	if (newWidget != null && newWidget.key is GlobalKey) {  
		final GlobalKey key = newWidget.key;  
		key._debugReserveFor(this);  
	}  
    return true;  
  }());  
  if (newWidget == null) {  
    if (child != null)  
      deactivateChild(child);  
    return null;  
  }  
  if (child != null) {  
    if (child.widget == newWidget) {  
      if (child.slot != newSlot)  
        updateSlotForChild(child, newSlot);  
    return child;  
  }  
  if (Widget.canUpdate(child.widget, newWidget)) {  
	if (child.slot != newSlot)  
        updateSlotForChild(child, newSlot);  
	child.update(newWidget);  
    assert(child.widget == newWidget);  
    assert(() {  
	  child.owner._debugElementWasRebuilt(child);  
	  return true;  
    }());  
    return child;  
  }  
  deactivateChild(child);  
  assert(child._parent == null);  
  }  
  return inflateWidget(newWidget, newSlot);  
}
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExOTUwODAxMTcsNzAzMTY0NzY2LC0xMD
A0NTg1NTkyLC0zMTk0NDIxNjVdfQ==
-->


## Trouble Shooting  
  
### Flutter Constaantly Rebuild  
  
to infer why flutter constantly rebuilds, consider following   
situation for further analysis.  
  
- #### ⚡ UniqueKey Wrongly Used:   
   uniqueley implicates that ___it is unique to itself___ which means its unique on   
    every rebuild, hence highly incorrespondent to StatefulWidget. If wrongly  
    used uniquekey to statefulWidget, it would force statefulWIdget to  
    recreate without saving any state...  
      
    __📝 example:__      
  > none...
  
  #### ⚡ Widget Constantly Changed on It's Child Visually  
  
  When parent doesn't have any visual constraints to it's  
    child, it's possible that some visually changed on child   
    could entail reconstruction of it's parent.  
  
    __📝 example:__  
    >A Container, which constantly change it's size, nested inside   
    another Container with no width been assigned to. This implicity  
    means the parent Container will forced to re-render on   
    constantly visually changed by it's child, since it doesn't  
    specify any width constraints to flutter.  
     >
    > 📝 Since width is unassigned in parent [Container], this would   
    force it's parent widget to re-render, hence rebuild occurs...  
      
	      
	```dart  
	Future.delayed(Duration(milliseconds: 500), (){  
		property_progress += 0.01; setState((){});  });  
		return Container(  
		 key: ValueKey('empty one...'), 
		 height: widget.height, 
		 /// width is unassigned here, this would force 
		 /// it's parent widget to re-render, hence rebuild 
		 /// occurs... 
		 // width: widget.width, 
		 color: Colors.blueGrey, 
		 alignment: AlignmentDirectional.centerStart, 
		 child: Container( 
			 height: widget.height, 
			 width: widget.width * min(property_progress, 1), 
			 color: MColors.random ));   
	```  
	
  __Solution:__  
  Assign width in parent [Container].  
      
	```dart  
	Future.delayed(Duration(milliseconds: 500), (){  
		property_progress += 0.01; setState((){});  });  
		return Container(  
		 key: ValueKey('empty one...'), 
		 height: widget.height, 
		 width: widget.width, 
		 color: Colors.blueGrey, 
		 alignment: AlignmentDirectional.centerStart, 
		 child: Container( 
			 height: widget.height, 
			 width: widget.width * min(property_progress, 1), 
			 color: MColors.random ));      
	```  
	
### Rendering Problems  
  
1) ### Accessing Size Information at Build Time  
	 __message:__  
	 
	 The following assertion was thrown building [WIDGET]  
	 <span style="color:blue">__Cannot get size during build.__</span>  
	         
	        ___Circumstance:___   
	> 1) pass newly created constraints from parent into child within build stage
	> 2) access context.size from build stage ?? 
	         
	       ___Solution___  
	 > 1) use layout builder to fetch constrains when you need to get  
	> size information dynamically  
	> 2) pass a constant constraints from parent into child  
          
2) ### LayoutBuilder Nested in IntrinsicHeight Widget

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTg2MjQzNzIxLDE0MDQ4NDUxOTEsLTExNT
ExNTY4MV19
-->

# NXP
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Authenticate And Protect](#authenticate-and-protect)
  - [Authenticate](#authenticate)
  - [Protect](#protect)
  - [Unprotect](#unprotect)
- [How Ntag I2C Demo / Tag be Initialized](#how-ntag-i2c-demo--tag-be-initialized)
- [Handling null Tag](#handling-null-tag)
- [Initialize Ntag_IC2_Command](#initialize-ntag_ic2_command)
- [Initialize MinimalNtag_IC2_Command](#initialize-minimalntag_ic2_command)
- [Ntag Demo MainActivity](#ntag-demo-mainactivity)
  - [onCreate](#oncreate)
  - [onPause](#onpause)
  - [onActivityResult](#onactivityresult)
  - [onNewIntent](#onnewintent)
  - [launchDemo](#launchdemo)
  - [launchNdefDemo](#launchndefdemo)
- [NtagDemo - NdefFragment](#ntagdemo---ndeffragment)
  - [onCreate](#oncreate-1)
  - [enableNdefReadLoop](#enablendefreadloop)
  - [readNdefClick](#readndefclick)
  - [writeNdefClick](#writendefclick)
- [Ntag_I2C_Demo](#ntag_i2c_demo)
  - [Properties and Constructor](#properties-and-constructor)
- [Constructor](#constructor)
  - [isConnected](#isconnected)
  - [isTagPresent](#istagpresent)
  - [isReady](#isready)
  - [getProduct](#getproduct)
  - [setBoardVersion](#setboardversion)
  - [resetTagMemory](#resettagmemory)
  - [getRegister_Settings](#getregister_settings)
  - [resetSessionRegisters](#resetsessionregisters)
- [Ntag_I2C_Demo - How New Intent be Processed](#ntag_i2c_demo---how-new-intent-be-processed)
- [Ntag_I2C_Demo - mermaid](#ntag_i2c_demo---mermaid)
  - [NDEF()](#ndef)
  - [NDEFReadTask()](#ndefreadtask)
  - [NDEFWrite](#ndefwrite)
  - [NDEFReadFinish](#ndefreadfinish)
  - [finishAllTasks](#finishalltasks)
  - [resetTagMemory](#resettagmemory-1)
  - [readSessionRegisters](#readsessionregisters)
  - [readWriteConfigRegister](#readwriteconfigregister)
  - [writeConfigRegisters](#writeconfigregisters)
  - [readTagContent](#readtagcontent)
  - [resetTagContent](#resettagcontent)
  - [LED](#led)
  - [LEDFinish](#ledfinish)
  - [ObtainAuthStatus()](#obtainauthstatus)
  - [Auth()](#auth)
  - [SRAMSpeedtest | SRAMSpeedtestTask (AsyncTask)](#sramspeedtest--sramspeedtesttask-asynctask)
  - [createNdefTextMessage](#createndeftextmessage)
  - [createNdefUriMessage](#createndefurimessage)
  - [createNdefBSSPMessage](#createndefbsspmessage)
  - [createNdefSpMessage](#createndefspmessage)
- [Ntag_I2C_Demo - How Demo be Initialized](#ntag_i2c_demo---how-demo-be-initialized)
- [How ReadEEPROM implements](#how-readeeprom-implements)
  - [in Ntag_I2C_Command](#in-ntag_i2c_command)
  - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command)
- [How ReadNDEF Implements](#how-readndef-implements)
  - [in Ntag_I2C_Command](#in-ntag_i2c_command-1)
  - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-1)
- [Reading Product Information](#reading-product-information)
  - [Ntag_I2C](#ntag_i2c)
  - [MinimalNtag_I2C](#minimalntag_i2c)
- [Reading configRegisters/sessionRegisters/...](#reading-configregisterssessionregisters)
  - [in Ntag_I2C_Command](#in-ntag_i2c_command-2)
  - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-2)
  - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-3)
  - [difference bewteen MinimalNtag and Ntag](#difference-bewteen-minimalntag-and-ntag)
  - [SR_Offset](#sr_offset)
- [How readSRAM implements](#how-readsram-implements)
- [How MFU(MifareUltralight) / NFCA transceive Command](#how-mfumifareultralight--nfca-transceive-command)
- [How transceive operates](#how-transceive-operates)
- [Wait for Read and Write](#wait-for-read-and-write)
- [How to Write EEPROM](#how-to-write-eeprom)
  - [in Ntag_I2C_Command](#in-ntag_i2c_command-3)
  - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-4)
  - [example on writeEEPROM listener](#example-on-writeeeprom-listener)
  - [Difference Between Ntag and Minimal Ntag](#difference-between-ntag-and-minimal-ntag)
- [How WriteNDEF Implements](#how-writendef-implements)
  - [in Ntag_I2C_Command](#in-ntag_i2c_command-4)
  - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-5)
  - [writeEEPROM listener](#writeeeprom-listener)
  - [CreateNDEFMessage](#createndefmessage)
- [How Write Auth | Config Registers Imlements](#how-write-auth--config-registers-imlements)
  - [Write Auth Registers](#write-auth-registers)
  - [Write Config Registers](#write-config-registers)
- [How Write SRAM Implements](#how-write-sram-implements)
  - [in Ntag_I2C_Command](#in-ntag_i2c_command-5)
  - [in MinimalNtag_I2C_Command](#in-minimalntag_i2c_command-6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->







## Authenticate And Protect

### Authenticate
#### Ntag_I2C_Command
##### authenticatePlus
Authenticate using PWD_AUTH command against NTAG I2C Plus product.

**describe writing process in brief**
- params
	- **byte[]** pwd :: 4Byte password to authenticate with
- check if its an **Ntag_I2C_Plus** product since only **Plus** support auth operation.
- read auth data by using reader.**pwdAuth**

**referenced in**
- Ntag_I2C_Demo
	- constructor
	- Auth
	- SRAMSpeedtestTask / doInBackground
	- EEPROMSpeedtestTask / doInBackground
- Ntag_I2C_Command  
	- writeSRAMBlock
	- readSRAMBlock
```java
@Override  
public byte[] authenticatePlus(byte[] pwd) throws IOException, NotPlusTagException {  
  if(getProduct() != Prod.NTAG_I2C_1k_Plus && getProduct() != Prod.NTAG_I2C_2k_Plus) {  
    throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
  }  
  return reader.pwdAuth(pwd);  
}
```
#### MinimalNtag_I2C_Command
##### authenticatePlus
Authenticate using PWD_AUTH command against NTAG I2C Plus product.
- ##### describe writing process in brief
	- params
		- **byte[]** pwd
			- 4Byte password to authenticate with
	- check if its an **Ntag_I2C_Plus** product since only **Plus** support auth operation.
	- perform auth operation by using **mfu.transceive**
		###### referenced in
		- Ntag_I2C_Demo
			- constructor
			- Auth
			- SRAMSpeedtestTask / doInBackground
			- EEPROMSpeedtestTask / doInBackground
		
```java
@Override  
public byte[] authenticatePlus(byte[] pwd) throws IOException, NotPlusTagException {  
	if(getProduct() != Prod.NTAG_I2C_1k_Plus && getProduct() != Prod.NTAG_I2C_2k_Plus) {  
		throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
	}  
	byte[] command = new byte[5];  
	command[0] = (byte) 0x1B;  
	command[1] = pwd[0];  
	command[2] = pwd[1];  
	command[3] = pwd[2];  
	command[4] = pwd[3];  
	return mfu.transceive(command);  
}
```

### Protect
#### Ntag_I2C_Command
##### protectPlus
Protect NTAG I2C Plus product memory map.
- ##### describe writing process in brief
	- params
		- **byte[]** pwd
			- 4Byte password to authenticate with
		- **byte** staretAddr
			- Page to lock the memory from
	- check if it's an **Ntag_I2C_Plus** product, since auth operation not supported by non Ntag_I2C_Plus 
	- reader.**SectorSelect** 0
	- set password indicated by user - using reader.**write**(pwd, Register.PWD)
	- write access configuration
	- write pt (pass thru) configuration
	- write auth0 configuration
	
		###### referenced in
		- Ntag_I2C_Demo
			- Auth
```java
	@Override  
	public void protectPlus(byte[] pwd, byte startAddr) throws IOException, FormatException, NotPlusTagException {  
		byte[] data = new byte[4];  

		if(getProduct() != Prod.NTAG_I2C_1k_Plus 
		&& getProduct() != Prod.NTAG_I2C_2k_Plus) {  
			throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
		}  
		reader.SectorSelect((byte) 0);  

		// Set the password indicated by the user  
		reader.write(pwd, Register.PWD.getValue());  

		byte access = (byte) 0x00;  
		byte authLimit = 0x00; // Don't limit the number of auth attempts  

		access ^= 1 << Access_Offset.NFC_PROT.getValue(); // NFC_Prot  
		access ^= 0 << Access_Offset.NFC_DIS_SEC1.getValue(); // NFC_DIS_SEC1  
		access |= authLimit << Access_Offset.AUTH_LIM.getValue(); // AUTHLIM  

		// Write the ACCESS configuration  data[0] = access;  
		data[1] = 0x00;  
		data[2] = 0x00;  
		data[3] = 0x00;  
		reader.write(data, Register.ACCESS.getValue());  

		byte ptI2C = 0x00;  
		byte i2CProt = 0x00;  

		ptI2C ^= 0 << PT_I2C_Offset.K2_PROT.getValue(); // 2K Prot  
		ptI2C ^= 1 << PT_I2C_Offset.SRAM_PROT.getValue(); // SRAM Prot  
		ptI2C |= i2CProt << PT_I2C_Offset.I2C_PROT.getValue(); // I2C Prot  

		// Write the PT_I2C configuration  data[0] = ptI2C;  
		data[1] = 0x00;  
		data[2] = 0x00;  
		data[3] = 0x00;  
		reader.write(data, Register.PT_I2C.getValue());  

		// Write the AUTH0 lock starting page  
		data[0] = 0x00;  
		data[1] = 0x00;  
		data[2] = 0x00;  
		data[3] = startAddr;  
		reader.write(data, Register.AUTH0.getValue());  
	}
```

#### MinimalNtag_I2C_Command
almost the same as Ntag_I2C, except using mfu.writePage


### Unprotect
behaviors almost the same as method of protect referred above.

#### Ntag_I2C_Command
#### MinimalNtag_I2C_Command
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjMxNzQ2MDk4XX0=
-->



## How Ntag I2C Demo / Tag be Initialized  
  
#### Ntag_I2C_demo  
Ntag_I2C_demo constructor  
```java  
public Ntag_I2C_Demo(Tag tag, final Activity main, final byte[] passwd, final int authStatus) {
}
```  
  
##### in AuthActivity  
- onCreate  
	```java  
	mTag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG); 
	if(mTag != null && Ntag_I2C_Demo.isTagPresent(mTag)) { 
		demo = new Ntag_I2C_Demo( 
			mTag, this,             
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());  
		} 
	 ``` 

- onNewIntent  
	```java  
		Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);
		demo = new Ntag_I2C_Demo( tag, this,             
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());  
	```  

- startDemo  
	```java  
		private void startDemo(Tag tag) {      
			// This authentication is added in order to avoid authentication problems with old NFC Controllers    
			if(MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {    
				demo.Auth(MainActivity.getPassword(), AuthStatus.Protected_RW.getValue());    
			}    
			if(demo != null && demo.isReady()) {    
				// Launch the thread    
				task = new authTask();    
				task.execute();    
			}    
		}  
	```  


##### in FlashMemoryActivity  
 - onNewIntent  
	```java  
		Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG); startDemo(tag, true); 
	``` 

- startDemo  
	```java  
		demo = new Ntag_I2C_Demo( tag, this,             
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());  
		if (!demo.isReady()) return; 
	``` 

##### MainActivity  
 - onCreate  
	```java  
		// Initialize the demo in order to handle tab change events 
		demo = new Ntag_I2C_Demo(null, this, null, 0); 
		mAdapter = NfcAdapter.getDefaultAdapter(this); 
		setNfcForeground(); 
		checkNFC(); 
	``` 

	- onNewIntent  
	```java  
		protected void onNewIntent(Intent nfc_intent) { 
			super.onNewIntent(nfc_intent); 
			// Set the pattern for vibration long pattern[] = { 0, 100 };            
			// Set the initial auth parameters  
			mAuthStatus = AuthStatus.Disabled.getValue(); 
			mPassword = null;            
			// Vibrate on new Intent  
			Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE); 		
			vibrator.vibrate(pattern, -1); 
			doProcess(nfc_intent); 
		} 
	``` 

	- doProcess  
	```java  
		public void doProcess(Intent nfc_intent) { 
			mIntent = nfc_intent; 
			Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG); 
			demo = new Ntag_I2C_Demo(tag, this, mPassword, mAuthStatus); 
			if (demo.isReady()) { 
				// Retrieve Auth Status before doing any operation 
				mAuthStatus = obtainAuthStatus(); 
				String currTab = mTabHost.getCurrentTabTag(); 
				launchDemo(currTab); 
			} 
		} 
	``` 


##### RegisterConfigActivity  
  - onCreate  
	```java  
		// Capture intent to check whether the operation should be automatically launch or not    
		Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);    
		if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {    
			startDemo(tag, false);    
		}    

		// Add Foreground dispatcher    
		mAdapter = NfcAdapter.getDefaultAdapter(this);    
		pendingIntent = PendingIntent.getActivity(  
			this, 0,      
			new Intent(this, getClass()
		).addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP), 0);    
		return; // end onCreate  
	```  

- startDemo  
	```java  
		private void startDemo(final Tag tag, boolean getAuthStatus) {      
			demo = new Ntag_I2C_Demo(tag, this,   
				MainActivity.getPassword(), 
				MainActivity.getAuthStatus());    
			if (!demo.isReady())    
				return;  
	```  

##### RegisterSessionActivity  
- onCreate  
	```java  
		// Capture intent to check whether the operation should be automatically launch or not    
		Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);    
		if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {    
			startDemo(tag, false);    
		}  
	```  

- startDemo  
	```java  
		private void startDemo(final Tag tag, boolean getAuthStatus) {      
			demo = new Ntag_I2C_Demo(tag, this,   
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());    
			if(!demo.isReady()) {    
				return;    
			}  
	```  

- onNewIntent  
	```java  
		protected void onNewIntent(Intent nfcIntent) {      
			// Set the initial auth parameters    
			MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());    
			MainActivity.setPassword(null);    

			// Store the intent information    
			MainActivity.setNfcIntent(nfcIntent);    
			Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG);    
			startDemo(tag, true);    
		}  
	```


##### ReadMemoryActivity  
- onNewIntent  
	```java  
		protected void onNewIntent(Intent nfcIntent) {      
			// Set the initial auth parameters    
			MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());    
			MainActivity.setPassword(null);    

			// Store the intent information    
			MainActivity.setNfcIntent(nfcIntent);    

			Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG);    
			startDemo(tag, true);    
		}  
	```  

	- startDemo  
	```java  
		private void startDemo(Tag tag, boolean getAuthStatus) {       
			// Complete the task in a new thread in order to be able to show the dialog    
			demo = new Ntag_I2C_Demo(  
				tag, this,        
				MainActivity.getPassword(),   
				MainActivity.getAuthStatus());    
			if(!demo.isReady()) {    
				return;    
			}    
		...  
	} 
	```


- ##### VersionActivity  
  the same as aboves



-----------


## Handling null Tag

```java
	// within onCreate...
	Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);    
	if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {    
		startDemo(tag, false);    
	}  
```

-----------------


## Initialize Ntag_IC2_Command
**Ntag_IC2_Command** is initialized on only one parameter. Unlike **MinimalNtag_I2C_Command** required two parameters, which is **Tag** tag and **Prod** tagType, since tagType in **Ntag_I2C_Command** can be fetched by **Ntag_Get_Version**.
1) **Tag** tag
```java
public static I2C_Enabled_Commands get(Tag tag) throws IOException, InterruptedException {  
	byte[] answer;  
	byte[] command = new byte[2];  
	NfcA nfca = NfcA.get(tag);  
	Prod prod;  

	// Check for support of setTimout to be able to send an efficient  
	// sector_select - select minimal implementation if not supported  nfca.setTimeout(20);  
	// check for timeout  
	if (nfca.getTimeout() < 50) {  
		// check if GetVersion is supported  
		try {  
			nfca.connect();  
			command = new byte[1];  
			command[0] = (byte) 0x60; // GET_VERSION  
			answer = nfca.transceive(command);  
			prod = (new Ntag_Get_Version(answer)).Get_Product();  
			nfca.close();  
			if (prod == Prod.NTAG_I2C_1k || prod == Prod.NTAG_I2C_2k  
			|| prod == Prod.NTAG_I2C_1k_T || prod == Prod.NTAG_I2C_2k_T  
			|| prod == Prod.NTAG_I2C_1k_V || prod == Prod.NTAG_I2C_2k_V  
			|| prod == Prod.NTAG_I2C_1k_Plus || prod == Prod.NTAG_I2C_2k_Plus) {  
				return new Ntag_I2C_Commands(tag);  
			}  
		} catch (Exception e) {  
			e.printStackTrace();  
			nfca.close();  

			// check if sector select is supported  
			try {  
				nfca.connect();  
				command = new byte[2];  
				command[0] = (byte) 0xC2; //SECTOR_SELECT  
				command[1] = (byte) 0xFF;  
				answer = nfca.transceive(command);  
				nfca.close();  
				return new Ntag_I2C_Commands(tag);  

			} catch (Exception e2) {  
				e.printStackTrace();  
				nfca.close();  
			}  
		}  
	}

```


-----------

## Initialize MinimalNtag_IC2_Command
**MinimalNtag_IC2_Command** is initialized by two parameters
1) **Tag** tag
2) **Prod** tagType (product)
	- generally speaking, tagType was fetched by Ntag_Get_Version(answer)
	- but there are some exceptions occurs in MininalNtag
  **todo: rewrite a new Ntag_Get_Version to consider those exceptions**
 
 
```java
	//check if we can use the minimal Version  
	MifareUltralight mfu = MifareUltralight.get(tag);  
	try {  
		mfu.connect();  
		command = new byte[1];  
		command[0] = (byte) 0x60; // GET_VERSION  
		answer = mfu.transceive(command);  
		prod = (new Ntag_Get_Version(answer)).Get_Product();  
		mfu.close();  
		if (prod == Prod.NTAG_I2C_1k || prod == Prod.NTAG_I2C_2k  
		|| prod == Prod.NTAG_I2C_1k_T || prod == Prod.NTAG_I2C_2k_T  
		|| prod == Prod.NTAG_I2C_1k_V || prod == Prod.NTAG_I2C_2k_V  
		|| prod == Prod.NTAG_I2C_1k_Plus || prod == Prod.NTAG_I2C_2k_Plus) {  
			return new MinimalNtag_I2C_Commands(tag, prod);  
		}  
	} catch (Exception e) {  
		e.printStackTrace();  
		mfu.close();  
		try {  
			mfu.connect();  
			answer = mfu.readPages(0);  
			// no exception is thrown so the phone can use the mfu.readPages  
			// function // also check if: // - tag is from NXP (byte 0 == 0x04) // - CC corresponds to a NTAG I2C 1K  
			if (answer[0] == (byte) 0x04 && answer[12] == (byte) 0xE1  
			&& answer[13] == (byte) 0x10 && answer[14] == (byte) 0x6D  
			&& answer[15] == (byte) 0x00) {  
				// check if Config is readable (distinguish from NTAG216), if  
				// not exception is thrown, and tag is not an // NTAG I2C 1k  answer = mfu.readPages(0xE8);  

				// Try to read session registers to differentiate between standard and PLUS products  
				try {  
					answer = mfu.readPages(0xEC);  
					mfu.close();  

					for (int i = 0; i < 4; i++) {  
						if (answer[i] != 0x00) {  
							prod = Prod.NTAG_I2C_1k_Plus;  
							return new MinimalNtag_I2C_Commands(tag, prod);  
						}  
					}  
					prod = Prod.NTAG_I2C_1k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				} catch (Exception e2) {  
					e2.printStackTrace();  
					mfu.close();  
					prod = Prod.NTAG_I2C_1k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				}  
			} else if (answer[0] == (byte) 0x04 && answer[12] == (byte) 0xE1  
				&& answer[13] == (byte) 0x10 && answer[14] == (byte) 0xEA  
				&& answer[15] == (byte) 0x00) {  
				// Try to read session registers to differentiate between standard and PLUS products  
				try {  
					answer = mfu.readPages(0xEC);  
					mfu.close();  
					for (int i = 0; i < 4; i++) {  
						if (answer[i] != 0x00) {  
							prod = Prod.NTAG_I2C_2k_Plus;  
							return new MinimalNtag_I2C_Commands(tag, prod);  
						}  
					}  
					prod = Prod.NTAG_I2C_2k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				} catch (Exception e2) {  
					e2.printStackTrace();  
					mfu.close();  
					prod = Prod.NTAG_I2C_2k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				}  
			} else {  
				mfu.close();  
				return new MinimalNtag_I2C_Commands(tag, Prod.NTAG_I2C_1k_Plus);  
			}  
		} catch (Exception e1) {  
			e1.printStackTrace();  
			mfu.close();  
		}  
	}  
	return new MinimalNtag_I2C_Commands(tag, Prod.NTAG_I2C_1k_Plus);

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMzE5OTc1MzhdfQ==
-->

## Ntag Demo MainActivity
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
authenticated --> demo.Auth-set_auth
demo.connected --> tag_detected
demo.connected --> demo.NDEF
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
eyJoaXN0b3J5IjpbNTQxMTE0Mjc0LC0yMDUwNDczNDc4LC0xOT
czMjY1MjM0LC0xNTYyNzc2NzYxLC0xNDEyOTI5NDI3LC05ODMw
MzU4MzEsNjc0OTU5MTc0LDEzNTM3NjY1NDMsNDkzODQwOF19
-->

## NtagDemo - NdefFragment
### onCreate
```mermaid
graph LR
!Write
WriteText
```
### enableNdefReadLoop
```mermaid
graph LR
demo.isReady --> demo.finishAllTask
demo.isReady --> launchNdefDemo
launchNdefDemo -.- password
launchNdefDemo -.- authStatus

```
```kotlin
fun enableNdefReadLoop(isChecked: Boolean){  
   if (isChecked == true && PseudoMainActivity.demo!!.isReady) {  
      if (PseudoMainActivity.demo == null)  
         throw ExceptionInInitializerError("demo should not be null")  
      val demo: Ntag_I2C_Demo = PseudoMainActivity.demo as Ntag_I2C_Demo;  
  
  demo.finishAllTasks()  
      if (PseudoMainActivity.password == null)  
         throw ExceptionInInitializerError("password should not be null")  
      PseudoMainActivity.launchNdefDemo(  
              PseudoMainActivity.authStatus,  
  PseudoMainActivity.password as ByteArray)  
   }  
}
```

### readNdefClick
```mermaid
graph LR
demo.NDEFreadFinish --> !Write
demo.NDEFreadFinish --> demo.isReady
demo.isReady --> demo.finishAllTask
demo.isReady --> lauchNdefDemo
lauchNdefDemo -.- password
lauchNdefDemo-.- authStatus
```
```kotlin
fun readNdefClick(){  
   if (PseudoMainActivity.demo == null)  
      throw ExceptionInInitializerError("demo should not be null")  
   val demo: Ntag_I2C_Demo = PseudoMainActivity.demo as Ntag_I2C_Demo;  
  
  demo.NDEFReadFinish()  
   val resources = registrar.activity().resources  
  ndefPerformance = resources.getString(R.string.layout_input_ndef_read)  
   ndefCallback = resources.getString(R.string.readNdefMsg)  
   isWriteChosen = false  
  
  // Read content  
  if (demo.isReady) {  
      demo.finishAllTasks()  
      if (PseudoMainActivity.password == null)  
         throw ExceptionInInitializerError("password should not be null")  
      PseudoMainActivity.launchNdefDemo(  
              PseudoMainActivity.authStatus,  
  PseudoMainActivity.password as ByteArray)  
   }  
}
```
### writeNdefClick
lauchNdefDemo (MainActivity) --> NdefFragment --> Demo.NDEF
```mermaid
graph LR
demo.NDEFReadFinish --> $Write
$Write --> demo.isReady
demo.isReady --> demo.finishAllTasks
demo.isReady --> $password
demo.isReady --> !password
$password --> lauchNdefDemo
lauchNdefDemo-.- authStatus
lauchNdefDemo-.- password
```
```kotlin
   fun writeNdefClick(){
		...
      if (PseudoMainActivity.demo == null)
         throw ExceptionInInitializerError("demo should not be null")
      val demo: Ntag_I2C_Demo = PseudoMainActivity.demo as Ntag_I2C_Demo;
      // Close the ReadNdef Taks
      demo.NDEFReadFinish()

      if (isWriteChosen == true) {
         if (demo.isReady) {
            demo.finishAllTasks()
            if (PseudoMainActivity.password == null)
               throw ExceptionInInitializerError("password should not be null")
            PseudoMainActivity.launchNdefDemo(
                    PseudoMainActivity.authStatus,
                    PseudoMainActivity.password as ByteArray)
        ....
```

different options
```mermaid
graph LR
RadioNdefText
RadioNdefUrl
RadioNdefBt
RadioNdefSp

NdefEditText
NdefEditLink

RadioNdefText --> NdefEditText
RadioNdefUrl --> NdefEditText
RadioNdefSp --> NdefEditLink

NdefEditLink -->|http://www|text_data
NdefEditText -->|..|text_data


```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MjMxOTk2NzMsMTcwOTE2MTU3OCw4Mj
UyNDIwODJdfQ==
-->


## Ntag_I2C_Demo 
### Properties and Constructor

## Constructor
#### describe process in brief
- params
	* tag **Tag** with which the Demos should be performed  
	* main **MainActivity**
	* passwd **byte[]**
	* authStatus **int**
- return null while tag is null
- initialize reader by **I2C_Enabled_Commands**.get(tag)
- connect reader
- get product info by reader.**getProduct**()

**Main Code**
```java
	public Ntag_I2C_Demo(Tag tag, final Activity main, final byte[] passwd, final int authStatus) {
		try {
			if (tag == null) {
				this.main = null;
				this.tag = null;
				return;
			}
			this.main = main;
			this.tag = tag;

			reader = I2C_Enabled_Commands.get(tag);

			if (reader == null) {
				String message = "The Tag could not be identified or this NFC device does not "
						+ "support the NFC Forum commands needed to access this tag";
				String title = "Communication failed";
				showAlert(message, title);
			} else {
				reader.connect();
			}

			Ntag_Get_Version.Prod prod = reader.getProduct();

			if (!prod.equals(Ntag_Get_Version.Prod.Unknown)) {
				if (prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_1k_Plus)
			     || prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_2k_Plus)) {
					// Auth status gets lost after resetting the demo when we
					// obtain the product we are dealing with
					if(authStatus == AuthStatus.Authenticated.getValue()) {
						reader.authenticatePlus(passwd);
					}
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
```

-----------------------------------------------------------

### isConnected
Checks if the tag is still connected based on the **previously** detected reader. Which means we already know what kind of reader has been detected. 
##### referenced in
- MainActivity
	- onCreate -> setOnTabChangedListener
	- launchNdefDemo
- Ntag | MinimalNtag
	- isConnected
- SpeedTestFragment
	-  StartEEPROMSpeedTest
	- StartSRAMSpeedTest
- UI (RegisterConfigActivity.onClick)
##### example in MainActivity.onCreate
```java
	if (tabId.equalsIgnoreCase("leds") && demo.isConnected()) {  
	  launchDemo(tabId);  
	}
```
##### example in StartXSpeedTest
```java
	if (MainActivity.demo.isReady() && MainActivity.demo.isConnected()) {
		...perform some task
```
**Main Code**
```java
public boolean isConnected() {
	// reader here indicates Ncfa ncfa.isConnected
	return reader.isConnected();
}
```

-----------------------------------------------------------
-----------------------------------------------------------


### isTagPresent
Checks if the tag is still connected based on the tag. Which means we don't know what kind of reader has been resolved. 
#### describe process in brief
- params
	- **Tag** tag 
- get Ndef by Ndef.get(tag) then perform following detection.
- detect if it's connected
	- ndef.isConnected()
	- nfca.isConnected()
	- nfcb.isConnected()
	-  nfcf.isConnected()
	- nfcv.isConnected()
- close ndef and return its result ( isConnected)

	##### referenced in
	- RegisterSessionActivity
	- ResetMemoryActivity
	- RegisterConfigActivity
	- AuthActivity
	##### example:
	```java
	// Capture intent to check whether the operation should be automatically launch or not  
	Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);  
	if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {  
		startDemo(tag, false);  
	}
	```
	
**Main Code**
```java
public static boolean isTagPresent(Tag tag) {
	final Ndef ndef = Ndef.get(tag);
	if (ndef != null && !ndef.getType().equals("android.ndef.unknown")) {
		try {
			ndef.connect();
			final boolean isConnected = ndef.isConnected();
			ndef.close();
			return isConnected;
		} catch (final IOException e) {
			e.printStackTrace();
			return false;
		}
	} else {
		final NfcA nfca = NfcA.get(tag);
		if (nfca != null) {
			try {
				nfca.connect();
				final boolean isConnected = nfca.isConnected();
				nfca.close();

				return isConnected;
			} catch (final IOException e) {
				e.printStackTrace();
				return false;
			}
		} else {
			final NfcB nfcb = NfcB.get(tag);
			if (nfcb != null) {
				try {
					nfcb.connect();
					final boolean isConnected = nfcb.isConnected();
					nfcb.close();
					return isConnected;
				} catch (final IOException e) {
					e.printStackTrace();
					return false;
				}
			} else {
				final NfcF nfcf = NfcF.get(tag);
				if (nfcf != null) {
					try {
						nfcf.connect();
						final boolean isConnected = nfcf.isConnected();
						nfcf.close();
						return isConnected;
					} catch (final IOException e) {
						e.printStackTrace();
						return false;
					}
				} else {
					final NfcV nfcv = NfcV.get(tag);
					if (nfcv != null) {
						try {
							nfcv.connect();
							final boolean isConnected = nfcv.isConnected();
							nfcv.close();
							return isConnected;
						} catch (final IOException e) {
							e.printStackTrace();
							return false;
						}
					} else {
						return false;
					}
				}
			}
		}
	}
}
```

-----------------------------------------------------------

### isReady
Checks if the demo is ready to be executed.
#### describe process in brief
- tag != null
- reader != null
```java
	public boolean isReady() {
		if (tag != null && reader != null) {
			return true;
		}
		return false;
	}
```
-----------------------------------------------------------
-----------------------------------------------------------


### getProduct
Get the product from detected reader.
```java
public Prod getProduct() throws IOException {
	return reader.getProduct();
}

class MyFlutterActivity : FlutterActivity() {  
  override fun onCreate(savedInstanceState: Bundle?) {  
    super.onCreate(savedInstanceState)  
    GeneratedPluginRegistrant.registerWith(this)  
  }  
}
```

-----------------------------------------------------------

### setBoardVersion
Set the current board version.
#### describe process in brief
- UI - onSetBoardVersion
	```java
	 public void onSetBoardVersion(String ver, String fwver){  
	  //todo: link to ui  
	  VersionInfoActivity.setBoardVersion(ver);  
	  VersionInfoActivity.setBoardFWVersion(fwver);  
	}
	```
##### referenced in
- VersionInfoActivity
	- onActivityResult
	- startDemo

-----------------------------------------------------------


### resetTagMemory
Resets the tag to its delivery values (including config registers).
#### describe functioning process in brief
* params
	* @return **int** indicating number of bytes written
	##### referenced in
	- ResetMemoryActivity
		- doInBackground
	 ##### example - statistic time to reset memory
	```java
	long regTimeOutStart = System.currentTimeMillis();  
	// Reset Tag demo will return the number of bytes written  
	int bytes = demo.resetTagMemory();  
	// Memory erase time statistics  
	timeToResetMemory = System.currentTimeMillis() - regTimeOutStart;  
	```
-----------------------------------------------------------


### getRegister_Settings
Builds a String array for the Registers.
#### describe process in brief
* params
	* register **[]byte** of the Registers 
	* @return **Ntag_I2C_Registers** ( a String Array )

##### referenced in
- Ntag_I2C_Demo
	- readSessionRegisters
	- readWriteConfigRegister
##### example



-----------------------------------------------------------


### resetSessionRegisters
Read out the session registers.
#### describe functioning process in brief
	```java

  
	```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2ODYxNDI3NDYsLTEyMTg3MjkxNDldfQ
==
-->



## Ntag_I2C_Demo - How New Intent be Processed
  
- ##### in AuthActivity  
```java
	protected void onNewIntent(Intent nfc_intent) {  
		super.onNewIntent(nfc_intent);  
		// Set the pattern for vibration  
		long pattern[] = { 0, 100 };  

		// Vibrate on new Intent  
		Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE);  
		vibrator.vibrate(pattern, -1);  

		// Get the tag and start the demo  
		Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);  
		demo = new Ntag_I2C_Demo(tag, 
			this, MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		MainActivity.setAuthStatus(demo.ObtainAuthStatus());  

		// This authentication is added in order to avoid authentication problems with old NFC Controllers  
		if(MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
			demo.Auth(MainActivity.getPassword(),
				 AuthStatus.Protected_RW.getValue());  
		}  

		// Set the Auth Status on the screen  
		updateAuthStatus(MainActivity.getAuthStatus());  
	}
```
- ##### MainActivity  
```java
	@Override  
	protected void onNewIntent(Intent nfc_intent) {  
	  super.onNewIntent(nfc_intent);  
	  // Set the pattern for vibration  
	  long pattern[] = { 0, 100 };  
	  
	  // Set the initial auth parameters  
	  mAuthStatus = AuthStatus.Disabled.getValue();  
	  mPassword = null;  
	  
	  // Vibrate on new Intent  
	  Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE);  
	  vibrator.vibrate(pattern, -1);  
	  doProcess(nfc_intent);  
	}

	public void doProcess(Intent nfc_intent) {  
		// m interpreted as mounted
		mIntent = nfc_intent;  
		Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);  
		demo = new Ntag_I2C_Demo(tag, this, mPassword, mAuthStatus);  
		if (demo.isReady()) {  
			// Retrieve Auth Status before doing any operation  
			mAuthStatus = obtainAuthStatus();  
			String currTab = mTabHost.getCurrentTabTag();  
			launchDemo(currTab);  
		}  
	}
	public static void setNfcIntent(Intent intent) {  
	   mIntent = intent;  
	}
```
- ##### RegisterConfigActivity  
```java
	protected void onNewIntent(Intent nfc_intent) {  
		// Set the initial auth parameters  
		MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());  
		MainActivity.setPassword(null);  

		// Set the Write protection check  
		isWriteProtected = false;  

		// Store the intent information  
		MainActivity.setNfcIntent(nfc_intent);  
		final Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);  
		startDemo(tag, true);  
	}
```
- ##### RegisterSessionActivity  
- ##### in FlashMemoryActivity  
- ##### VersionActivity  
- ##### ReadMemoryActivity  
- ##### ResetMemeoryActivity
```java
	protected void onNewIntent(Intent nfcIntent) {  
		// Set the initial auth parameters  
		MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());  
		MainActivity.setPassword(null);  

		// Store the intent information  
		MainActivity.setNfcIntent(nfcIntent);  

		// Complete the task in a new thread in order to be able to show the dialog  
		Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG);  
		startDemo(tag, true);  
	}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk1ODI1MzQ0N119
-->



## Ntag_I2C_Demo - mermaid

### NDEF()
- read - by NDEFReadTask
- write - by NDEFWrite
``` mermaid
graph LR
 
$Write
!Write
$addAar

subgraph NdefFragment - WriteOptions
	NdefText
	NdefUri
	NdefBT
	NdefSp
	ndefType
end


$Write --> NDEFWrite
$Write --> ndefType
$Write --> $addAar
!Write --> NDEFReadTask

ndefType --> NdefText
ndefType --> NdefUri
ndefType --> NdefBT
ndefType --> NdefSp

NdefText -.- createNdefTextMessage
NdefUri -.- createNdefUriMessage
NdefBT -.- createNdefBSSPMessage
NdefSp -.- createNdefSpMessage
createNdefTextMessage --> msg0
createNdefUriMessage --> msg0
createNdefBSSPMessage --> msg0
createNdefSpMessage --> msg0

$addAar --> NdefRecord.createAar
NdefRecord.createAar -.- packageName
NdefRecord.createAar --> aarRecord

aarRecord --> msg
msg -.- NDEFWrite
msg0 -.- NDEFWrite

```
msg: **NDEFMessage** | write: **NdefFragment.Boolean** 
NDEFWrite: 在這裡將 AAR 寫入 NdefRecords 中的第二筆 (android官方建議寫入第二筆)

```kotlin
public void NDEF() throws IOException {
// Check if the operation is read or write
if (NdefFragment.isWriteChosen()) {
	// NDEF Message to write in the tag
	NdefMessage msg = null;

	// Get the selected NDEF type since the creation of the NDEF Msg
	// will vary depending on the type
	if (NdefFragment.getNdefType().equalsIgnoreCase(...)) {
		msg = createNdefTextMessage(NdefFragment.getText());
	} else if (NdefFragment.getNdefType().equalsIgnoreCase(...))) {
		msg = createNdefUriMessage(NdefFragment.getText());
	} else if (NdefFragment.getNdefType().equalsIgnoreCase(...))) {
		msg = createNdefBSSPMessage();
	} else if (NdefFragment.getNdefType().equalsIgnoreCase(...))) {
		msg = createNdefSpMessage(NdefFragment.getSpTitle(), NdefFragment.getSpLink());
	}
	if(msg == null) {
		toastText(main, "Please add correct input values", Toast.LENGTH_LONG) ;
		NdefFragment.setAnswer(...error...);
	}
	if(NdefFragment.isAarRecordSelected()) {
		NdefRecord aarRecord = NdefRecord.createApplicationRecord(PseudoMainActivity.getPACKAGE_NAME());

          assert msg != null;
          NdefRecord records[] = msg.getRecords();
		records  = Arrays.copyOf(records, records.length + 1);
		records[records.length - 1] = aarRecord;
		msg = new NdefMessage(records);
	}
	try {
		long timeToWriteNdef = NDEFWrite(msg);
		toastText(main, "write tag successfully done", Toast.LENGTH_LONG) ;
		NdefFragment.setAnswer("Tag successfully written");

		assert msg != null;
		int bytes = msg.toByteArray().length;

		String Message = "";

		// Transmission Results
		Message = Message.concat("Speed (" + bytes + " Byte / "
				+ timeToWriteNdef + " ms): "
				+ String.format("%.0f", bytes / (timeToWriteNdef / 1000.0))
				+ " Bytes/s");
		NdefFragment.setDatarate(Message);
	} catch (Exception e) {
		toastText(main, "write tag failed", Toast.LENGTH_LONG) ;
		NdefFragment.setDatarate("Error writing NDEF");
		e.printStackTrace();
	}
} else {
	ndefreadtask = new NDEFReadTask();
	ndefreadtask.execute();
}
}
```


### NDEFReadTask()
```mermaid
graph LR

execute
doInBackground
onProgressUpdate
onPostExecute
execute --> onPreExecute
onPreExecute --> doInBackground
doInBackground -.- onProgressUpdate
doInBackground --> onPostExecute
```

**doInBackground**
```mermaid
graph LR
msg

readNDEF --> readTime
readNDEF --> msg
Message -.- msg
Message --> hightLevelMsg
hightLevelMsg -->|highLevelMsg.get| type
hightLevelMsg--> EmptyRecord
hightLevelMsg--> SmartPosterRecord
hightLevelMsg--> TextRecord
hightLevelMsg--> AARecord
hightLevelMsg--> MIMERecord

EmptyRecord --> message
SmartPosterRecord--> message
TextRecord--> message
AARecord--> message
MIMERecord--> message
```
```mermaid
graph LR
	readTime
	type
	publishProgress -.- message
	publishProgress -.- readTime
	publishProgress -.- type
```
> readNDEF: **reader** | Message: **NDEFTool.Message** | type: **String - NdefType** | msg: **NDEFMessage** | message: **String - NdefMessage** | readTime: **Long - datarate**


### NDEFWrite
只call了read.writeNDEF, 並計算時間
```mermaid
graph LR
timeStart
timeStart --> reader.writeNDEF
reader.writeNDEF -.- msg
msg --> timeEnd
timeStart --> readTime
timeEnd --> readTime
```
msg: **NDEFMessage**


### NDEFReadFinish
```mermaid
graph LR
NdefFragment.resetNdefDemo
```

```kotlin
public void NDEFReadFinish() {
	if (ndefreadtask != null && !ndefreadtask.isCancelled()) {
		ndefreadtask.exit = true;
		try {
			ndefreadtask.get();
		} catch (Exception e) {
			e.printStackTrace();
		}
		ndefreadtask = null;
		// Clean all the fields
		NdefFragment.resetNdefDemo();
	}
}
```


### finishAllTasks
```mermaid
graph LR
	LEDFinish
	SRAMSpeedFinish
	EEPROMSpeedFinish
	WriteEmptyNdefFinish
	NDEFReadFinish
```


### resetTagMemory
``` mermaid
graph LR
Ntag
MiniNtag
bytesWritten
equal-0
greater-0
bytesWritten --> equal-0
bytesWritten --> greater-0
greater-0 -.-Ntag
equal-0 -.- MiniNtag
MiniNtag --> Alert

Ntag --> writeConfigRegisters
Ntag --> 1K2KPlus
1K2KPlus --> writeAuthRegisters
```
```kotlin
public int resetTagMemory() {
	int bytesWritten = 0;

	try {
		bytesWritten = reader.writeDeliveryNdef();
	} catch (Exception e) {
		e.printStackTrace();
		bytesWritten = -1;
	}
	if(bytesWritten == 0) {
		showDemoNotSupportedAlert();
	} else {
		byte NC_REG = (byte) 0x01;
		byte LD_Reg = (byte) 0x00;
		byte SM_Reg = (byte) 0xF8;
		byte WD_LS_Reg = (byte) 0x48;
		byte WD_MS_Reg = (byte) 0x08;
		byte I2C_CLOCK_STR = (byte) 0x01;
		// If we could reset the memory map, we should be able to write the config registers
		try {
			reader.writeConfigRegisters(NC_REG, LD_Reg,
					SM_Reg, WD_LS_Reg, WD_MS_Reg, I2C_CLOCK_STR);
		} catch (Exception e) {
			//Toast.makeText(main, "Error writing configuration registers", Toast.LENGTH_LONG).show();
			toastText(main, "Error writing configuration registers", Toast.LENGTH_LONG);
			e.printStackTrace();
			bytesWritten = -1;
		}

		try {
			Ntag_Get_Version.Prod prod = reader.getProduct();

			if (prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_1k_Plus)
			 || prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_2k_Plus)) {
				byte AUTH0 = (byte) 0xFF;
				byte ACCESS = (byte) 0x00;
				byte PT_I2C = (byte) 0x00;
				reader.writeAuthRegisters(AUTH0, ACCESS, PT_I2C);
			}
		} catch (Exception e) {
			//Toast.makeText(main, "Error writing authentication registers", Toast.LENGTH_LONG).show();
			toastText(main, "Error writing authentication registers", Toast.LENGTH_LONG);
			e.printStackTrace();
			bytesWritten = -1;
		}
	}
	return bytesWritten;
}
```

### readSessionRegisters
```mermaid
graph LR
reader.getSessionRegisters --> reg
getRegister_Settings -.- reg
getRegister_Settings --> Ntag_I2C_Registers
RegisterSessionActivity.SetAnswer -.- Ntag_I2C_Registers
showToast
```
> RegisterSessionActivity.SetAnswer -> turn **Ntag_I2C_Registers** into readable map
>  getRegister_Settings(byte[]) -> **Ntag_I2C_Registers**
```kotlin
public void readSessionRegisters() throws CommandNotSupportedException {
	try {
		byte[] sessionRegisters = reader.getSessionRegisters();
		Ntag_I2C_Registers answer = getRegister_Settings(sessionRegisters);
		RegisterSessionActivity.SetAnswer(answer, main);
		toastText(main, "read tag successfully done", Toast.LENGTH_LONG) ;
	} catch (CommandNotSupportedException e) {
		e.printStackTrace();
		throw e;
	} catch (Exception e) {
		e.printStackTrace();
		toastText(main, "read tag failed", Toast.LENGTH_LONG) ;
	}
}
```

### readWriteConfigRegister
```mermaid
graph LR
$Write
!Write
$1k2kPlus+Auth
!1k2kPlus+Auth

$Write --> $1k2kPlus+Auth
$1k2kPlus+Auth --> Alert
$Write --> !1k2kPlus+Auth
!1k2kPlus+Auth --> writeConfigRegisters

!Write --> $plus 
$plus --> reader.getConfigRegisters
reader.getConfigRegisters -.- getRegister_Settings
getRegister_Settings --> Ntag_I2C_Registers
RegisterConfigActivity.setAnswer -.- Ntag_I2C_Registers

subgraph RegisterConfigActivity
RegisterConfigActivity.setAnswer -.- NC_Reg
RegisterConfigActivity.setAnswer -.- LD_Reg
RegisterConfigActivity.setAnswer -.- SM_Reg
RegisterConfigActivity.setAnswer -.- NS_Reg
RegisterConfigActivity.setAnswer -.- WD_LS_Reg
RegisterConfigActivity.setAnswer -.- WD_MS_Reg
RegisterConfigActivity.setAnswer -.- I2C_CLOCK_STR
end

!Write --> !plus
!plus --> reader.getAuth0Register
!plus --> reader.getAccessRegister
!plus --> reader.getPTI2CRegister
subgraph RegisterConfigActivity
	reader.getAuth0Register -.- setAuth0
	reader.getAccessRegister -.- setAcess
	reader.getPTI2CRegister -.- setPTI2C
end
reader.getAuth0Register -.- Ntag_I2C_Plus_Registers
reader.getAccessRegister-.- Ntag_I2C_Plus_Registers
reader.getPTI2CRegister-.- Ntag_I2C_Plus_Registers

Ntag_I2C_Registers --> updateToDart
Ntag_I2C_Plus_Registers --> updateToDart
```
> Write: **RegisterConfigActivity.Boolean**
> 1k2kPlus: **Ntag_Get_version.Prod** via reader.getProduct
> 1k2kPlus+Auth: **RegisterConfigActivity.getAuth0()**
> reader.getConfigRegisters: **byte[]**
> getRegister_Settings(byte[]): **Ntag_I2C_Registers**

```kotlin
public void readWriteConfigRegister() throws CommandNotSupportedException {
	// Check if the operation is read or write
	if (RegisterConfigActivity.isWriteChosen()) {
		try {
			Ntag_Get_Version.Prod prod = reader.getProduct();
			if((prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_1k_Plus)
			 || prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_2k_Plus))
			 && (RegisterConfigActivity.getAuth0() & 0xFF) <= 0xEB) {
				showAuthWriteConfigAlert();
			 } else {
				writeConfigRegisters();
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	} //END if get chosen
	else {
		try {
			byte[] configRegisters = reader.getConfigRegisters();

			Ntag_I2C_Registers answer = getRegister_Settings(configRegisters);
			RegisterConfigActivity.setAnswer(answer);
			RegisterConfigActivity.setNC_Reg(configRegisters[CR_Offset.NC_REG.getValue()]);
			RegisterConfigActivity.setLD_Reg(configRegisters[CR_Offset.LAST_NDEF_PAGE.getValue()]);
			RegisterConfigActivity.setSM_Reg(configRegisters[CR_Offset.SM_REG.getValue()]);
			RegisterConfigActivity.setNS_Reg(configRegisters[CR_Offset.REG_LOCK.getValue()]);
			RegisterConfigActivity.setWD_LS_Reg(configRegisters[CR_Offset.WDT_LS.getValue()]);
			RegisterConfigActivity.setWD_MS_Reg(configRegisters[CR_Offset.WDT_MS.getValue()]);
			RegisterConfigActivity.setI2C_CLOCK_STR(configRegisters[CR_Offset.I2C_CLOCK_STR.getValue()]);

			Ntag_Get_Version.Prod prod = reader.getProduct();
			if (prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_1k_Plus)
			 || prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_2k_Plus)) {
				byte[] auth0Register = reader.getAuth0Register();
				byte[] accessRegister = reader.getAccessRegister();
				byte[] pti2cRegister = reader.getPTI2CRegister();

				Ntag_I2C_Plus_Registers answerPlus = getPlusAuth_Settings(auth0Register, accessRegister, pti2cRegister);
				RegisterConfigActivity.setAnswerPlus(answerPlus);
				RegisterConfigActivity.setAuth0(auth0Register[3]);
				RegisterConfigActivity.setAccess(accessRegister[0]);
				RegisterConfigActivity.setPtI2C(pti2cRegister[0]);
			}
			//grodianknot
			RegisterConfigActivity.updateToDart();
			toastText(main, "read tag successfully done",
					Toast.LENGTH_LONG) ;
		} catch (CommandNotSupportedException e) {
			e.printStackTrace();
			throw e;
		} catch (Exception e) {
			e.printStackTrace();
			toastText(main, "read tag failed", Toast.LENGTH_LONG);
		}
	}
}
```

### writeConfigRegisters

```mermaid
graph LR
$1k2kPlus


subgraph RegisterConfigActivity
	NC_Reg
	LD_Reg
	SM_Reg
	WD_LS_Reg
	WD_MS_Reg
	I2C_CLOCK_STR
end
reader.writeConfigRegisters -.- NC_Reg
reader.writeConfigRegisters -.- LD_Reg
reader.writeConfigRegisters -.- SM_Reg
reader.writeConfigRegisters -.- WD_LS_Reg
reader.writeConfigRegisters -.- WD_MS_Reg
reader.writeConfigRegisters -.- I2C_CLOCK_STR

subgraph __RegisterConfigActivity__
	AUTH0 -.- getAuth0
	ACCESS -.- getAccess
	PT_I2C -.- getPtI2C
end
$1k2kPlus --> reader.writeAuthRegisters
reader.writeAuthRegisters -.- AUTH0
reader.writeAuthRegisters -.- ACCESS
reader.writeAuthRegisters -.- PT_I2C
```
> reader.writeAuthRegisters
>    - write default password (0xFF 0xFF 0xFF 0xFF)
>     
```kotlin
private void writeConfigRegisters() {
	try {
		byte NC_Reg = (byte) RegisterConfigActivity.getNC_Reg();
		byte LD_Reg = (byte) RegisterConfigActivity.getLD_Reg();
		byte SM_Reg = (byte) RegisterConfigActivity.getSM_Reg();
		byte WD_LS_Reg = (byte) RegisterConfigActivity.getWD_LS_Reg();
		byte WD_MS_Reg = (byte) RegisterConfigActivity.getWD_MS_Reg();
		byte I2C_CLOCK_STR = (byte) RegisterConfigActivity.getI2C_CLOCK_STR();
		reader.writeConfigRegisters(NC_Reg, LD_Reg, SM_Reg, WD_LS_Reg, WD_MS_Reg, I2C_CLOCK_STR);
		Ntag_Get_Version.Prod prod = reader.getProduct();
		if (prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_1k_Plus)
		 || prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_2k_Plus)) {
			byte AUTH0 = (byte) RegisterConfigActivity.getAuth0();
			byte ACCESS = (byte) RegisterConfigActivity.getAccess();
			byte PT_I2C = (byte) RegisterConfigActivity.getPtI2C();
			reader.writeAuthRegisters(AUTH0, ACCESS, PT_I2C);
		}
		toastText(main, "write tag successfully done",	Toast.LENGTH_LONG) ;
	} catch (Exception e) {
		e.printStackTrace();
		toastText(main, "write tag failed", Toast.LENGTH_LONG) ;
}
}
```

### readTagContent
Reads the whole tag memory content., return  Boolean indicating success or error
- reader.readEEPROM (start, end)

referenced in ReadMemoryActivity


```mermaid
graph LR
reader.getProduct --> memsize 
memsize -.- reader.readEEPROM
reader.getBlockSize -.- reader.readEEPROM
```
```kotlin
public byte[] readTagContent() {
	byte[] bytes = null;
	try {
		// The user memory and the first four pages are displayed
		int memSize = reader.getProduct().getMemsize() + 16;
		// Read all the pages using the fast read method
		bytes = reader.readEEPROM(0, memSize / reader.getBlockSize());
	} catch (IOException e) {
		e.printStackTrace();
	} catch (FormatException e) {
		e.printStackTrace();
	} catch (CommandNotSupportedException e) {
		e.printStackTrace();
		showDemoNotSupportedAlert();
	} catch (Exception e) {
		e.printStackTrace();
	}
	return bytes;
}
```

### resetTagContent

```mermaid
graph LR
reader.getProduct --> memsize
memsize --> EmptyBytesOfMemsize
reader.writeEEPROM -.- EmptyBytesOfMemsize
```
```kotlin
public boolean resetTagContent() {
	boolean success = true;
	try {
		byte[] d = new byte[reader.getProduct().getMemsize()];
		reader.writeEEPROM(d, this);
	} catch (IOException e) {
		success = false;
		e.printStackTrace();
	} catch (FormatException e) {
		success = false;
		e.printStackTrace();
	} catch (CommandNotSupportedException e) {
		showDemoNotSupportedAlert();
		e.printStackTrace();
	}
	return success;
}
```


### LED
Performs the LED Demo
```mermaid
graph LR
subgraph LedFragment
	setAnswer -.- defaults
	setTemperatureC -.- default0
	setTemperatureF -.- default0
	setVoltage -.- default0
end

LedTask
```

```kotlin
public void LED() throws IOException, FormatException {
	// Reset UI
	LedFragment.setAnswer(main.getResources().getString(R.string.readConf));
	LedFragment.setTemperatureC(0);
	LedFragment.setTemperatureF(0);
	LedFragment.setVoltage(0);

	// The demo is executed in a separate thread to let the GUI run
	lTask = new LedTask();
	lTask.execute();
}
```

### LEDFinish
```mermaid

```

```kotlin
public void LEDFinish() {
	if (lTask != null && !lTask.isCancelled()) {
		lTask.exit = true;
		try {
			lTask.get();
		} catch (Exception e) {
			e.printStackTrace();
		}
		lTask = null;
	}
}
```


### ObtainAuthStatus()
Retrieves the auth status of the tag
- reader.getProductionPlus
- AuthStatus.Disable
``` mermaid
graph LR
!1k2kPlus --> AuthStatus.Disable
$1k2kPlus --> reader.getProtectionPlus

```
```kotlin
public int ObtainAuthStatus() {
	try {
		Ntag_Get_Version.Prod prod = reader.getProduct();
		if (!prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_1k_Plus)
		 && !prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_2k_Plus)) {
			return AuthStatus.Disabled.getValue();
		} else {
			return reader.getProtectionPlus();
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
	return AuthStatus.Disabled.getValue();
}
```


### Auth()
Performs the authentication operation on NTAG I2C Plus 

- pwd **ByteArray** containing the password  
- authStatus Current Authentication Status  

perform authentication on Tag
- reader.authenticate -> reader.pwdAuth
- reader.unprotectPlus
- reader.protectPlus
```mermaid
graph LR
subgraph AuthStatus
	unprotected
	authenticated
	W_RW
	authStatus --> W_RW
	authStatus --> authenticated
	authStatus --> unprotected
end

unprotected --> reader.protectPlus
reader.protectPlus -.- pwd
reader.protectPlus -.- Ntag_I2C_Commands.Register
authenticated --> reader.unprotectPlus
W_RW --> reader.authenticatePlus
reader.authenticatePlus --> reader.pwdAuth
reader.pwdAuth -.- pwd

Protected_W -.- Protected_RW
Protected_RW -.- Protected_W_SRAM
Protected_W_SRAM -.- Protected_RW_SRAM
Protected_W --> W_RW
```
>reader.protectPlus (**byte[]** pwd, **byte[]** startAddr)
	>   - write user password

>reader.unprotectPlus
>   - write default password ( 0xFF, 0xFF, 0xFF, 0xFF )

>reader.authenticatePlus

```kotlin
public Boolean Auth(byte[] pwd, int authStatus) {
	try {
		if(authStatus == AuthStatus.Unprotected.getValue()) {
			reader.protectPlus(pwd, Register.Capability_Container.getValue());
		} else if(authStatus == AuthStatus.Authenticated.getValue()) {
			reader.unprotectPlus();
		} else if(authStatus == AuthStatus.Protected_W.getValue()
				|| authStatus == AuthStatus.Protected_RW.getValue()
				|| authStatus == AuthStatus.Protected_W_SRAM.getValue()
				|| authStatus == AuthStatus.Protected_RW_SRAM.getValue()) {
			byte[] pack = reader.authenticatePlus(pwd);
			if(pack.length < 2) {
				return false;
			}
		}
		return true;
	} catch (IOException e) {
		e.printStackTrace();
	} catch (FormatException e) {
		e.printStackTrace();
	} catch (NotPlusTagException e) {
		showTagNotPlusAlert();
		e.printStackTrace();
	}
	return false;
}
```

example usage
```kotlin
// Authenticate in order to let the user use the demos
demo!!.Auth(PseudoMainActivity.password, AuthStatus.Protected_RW.value)

// Perform auth operation based on the actual status  
demo!!.Auth(PseudoMainActivity.password,  
  PseudoMainActivity.authStatus)
```



### SRAMSpeedtest | SRAMSpeedtestTask (AsyncTask)
... todo
```mermaid
graph LR
	execute
	doInBackground
	onProgressUpdate
	onPostExecute
	execute --> onPreExecute
	onPreExecute --> doInBackground
	doInBackground -.- onProgressUpdate
	doInBackground --> onPostExecute
```
```mermaid
graph LR
$1k2kPlus --> $Authenticated
$1k2kPlus --> !Authenticated
$Authenticated --> reader.authenticatePlus.pw.
```
```kotlin
 
```
### createNdefTextMessage
```mermaid
text-bytes
lang-bytes
text-length
lang-length
```
### createNdefUriMessage
### createNdefBSSPMessage
### createNdefSpMessage
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzIwMzQ0NDE5LDgwNTQ2MTAyNSwtMTA5Nz
I5ODYwNSw0NDcxMzkwMDgsMTYwMjc4MTAwOCwxNTk3NTExMTQz
LDEwMzM0MDU2NTMsMTEwMjQ3MzU5NSwxMTE4MzU5NTYxLC02NT
U5ODU0OCwtNDk4MTExNjE5LC0xNzkwOTg5ODA2LC0xNzkxNTYy
MzE5LC04NTczMjA0NTUsLTExMTA5ODk3MzEsLTExNTc5MDY5Mz
EsLTE3ODQ3OTU4MjhdfQ==
-->



## Ntag_I2C_Demo - How Demo be Initialized

- ##### in AuthActivity
```java
	private void startDemo(Tag tag) {  
		// This authentication is added in order to avoid authentication problems with old NFC Controllers  
		if(MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
			demo.Auth(MainActivity.getPassword(), AuthStatus.Protected_RW.getValue());  
		}  
		if(demo != null && demo.isReady()) {  
			// Launch the thread  
			task = new authTask();  
			task.execute();  
		}  
	}
```
- ##### in FlashMemoryActivity
```java
	private void startDemo(Tag tag, boolean getAuthStatus) {  
		try {  
			if(isAppFW) {  
				bytesToFlash = readFileAssets(indexFW);  
			} else {  
				String path = ((TextView) findViewById(R.id.file_path)).getText().toString();  
				bytesToFlash = readFileMemory(path);  
			}  
		} catch (IOException e) {  
			e.printStackTrace();  

			// Set bytesToFlash to null so that the task is not started  
			bytesToFlash = null;  
		}  
	
		if(bytesToFlash == null || bytesToFlash.length == 0) {  
			Toast.makeText(mContext, "Error could not open File",  
			Toast.LENGTH_SHORT).show();  
			return;  
		}  

		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		if (!demo.isReady())  
			return;  

		// Retrieve the Auth Status  
		if(getAuthStatus)  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  

		// Flashing is only available when the tag is not protected  
		if(MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
			// Launch the thread  
			task = new flashTask();  
			task.execute();  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### MainActivity 
```java

```
- ##### RegisterConfigActivity 
```java
	private void startDemo(final Tag tag, boolean getAuthStatus) {  
	    demo = new Ntag_I2C_Demo(tag, this, 
		    MainActivity.getPassword(), 
		    MainActivity.getAuthStatus());  
		if (!demo.isReady())  
			return;  

		// Calculate the Register Values according to what has been selected by  
		// the user  calcConfiguration();  

		// Retrieve the Auth Status  
		if (getAuthStatus == true) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		if (MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W_SRAM.getValue()) {  
			try {  
				demo.readWriteConfigRegister();  
			} catch (CommandNotSupportedException e) {  
				new AlertDialog.Builder(this)  
					.setMessage("This NFC device does not support the NFC Forum commands needed to access the config register")  
					.setTitle("Command not supported")  
					.setPositiveButton("OK",  
						new DialogInterface.OnClickListener() {  
							@Override  
							public void onClick(DialogInterface dialog, int which) {  

							}  
						}).show();  
				return;  
			}  
			layout_read.setVisibility(View.GONE);  
			layout_buttons.setVisibility(View.VISIBLE);  
			scroll_regs.setVisibility(View.VISIBLE);  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### RegisterSessionActivity 
```java
	private void startDemo(final Tag tag, boolean getAuthStatus) {  
		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		if (!demo.isReady())  
			return;  

		// Calculate the Register Values according to what has been selected by  
		// the user  calcConfiguration();  

		// Retrieve the Auth Status  
		if (getAuthStatus == true) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		if (MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W_SRAM.getValue()) {  
			try {  
				demo.readWriteConfigRegister();  
			} catch (CommandNotSupportedException e) {  
				new AlertDialog.Builder(this)  
					.setMessage(  
					"This NFC device does not support the NFC Forum commands needed to access the config register")  
					.setTitle("Command not supported")  
					.setPositiveButton("OK", new DialogInterface.OnClickListener() {  
						@Override  
						public void onClick(DialogInterface dialog,  
						int which) {  

						}  
					}).show();  
				return;  
			}  
			layout_read.setVisibility(View.GONE);  
			layout_buttons.setVisibility(View.VISIBLE);  
			scroll_regs.setVisibility(View.VISIBLE);  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### ReadMemoryActivity 
```java
	private void startDemo(Tag tag, boolean getAuthStatus) {  
		// Complete the task in a new thread in order to be able to show the dialog  
		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		if(!demo.isReady()) {  
			return;  
		}  

		// Retrieve the Auth Status  
		if(getAuthStatus) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		if(MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W_SRAM.getValue()) {  
		// Launch the thread  
			new readTask().execute();  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### VersionActivity 
```java

```
- ##### ResetMemeoryActivity
```java
	private void startDemo(Tag tag, boolean getAuthStatus) {  
		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(),  
			MainActivity.getAuthStatus());  
		if(!demo.isReady()) {  
			return;  
		}  

		// Retrieve the Auth Status  
		if(getAuthStatus) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		// Demo is only available when the tag is not protected  
		if(MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
		// Launch the thread  
			new resetTask().execute();  
		} else {  
			showAuthDialog();  
		}  
	}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkzMDkxNTkyMV19
-->





##  How ReadEEPROM implements
### in Ntag_I2C_Command
#### readEEPROM
Read Data from the EEPROM.
- params
	- **int** absStart, Start of the read
	- **int** absEnd, End of the read(included in the Answer)

	###### referenced in
	- MinimalNtag_I2C_Command | Ntag_I2C_Command
		- readNDEF
	- Ntag_I2C_Demo
		- doInBackground
		- readTagContent
	 
```java
	@Override  
	public byte[] readEEPROM(int absStart, int absEnd) throws IOException,FormatException {  
		int maxfetchsize = reader.getMaxTransceiveLength();  
		int maxFastRead = (maxfetchsize - 2) / 4;  
		int fetchStart = absStart;  
		int fetchEnd = 0;  
		byte[] data = null;  
		byte[] temp = null;  

		reader.SectorSelect((byte) 0);  

		while (fetchStart <= absEnd) {  
			... see detail 
		}  
		// Let's go back to Sector 0  
		reader.SectorSelect((byte) 0);  
		return data;  
	}	
```



### in MinimalNtag_I2C_Command
#### readEEPROM
Read Data from the EEPROM.
- params
	- **int** absStart, Start of the read
	- **int** absEnd, End of the read(included in the Answer)

	###### referenced in
	- MinimalNtag_I2C_Command | Ntag_I2C_Command
		- readNDEF
	- Ntag_I2C_Demo
		- doInBackground
		- readTagContent
	 
```java
	@Override  
	public byte[] readEEPROM(int absStart, int absEnd) throws IOException,  
		FormatException, CommandNotSupportedException {  
		if ((tagType == Prod.NTAG_I2C_2k && absEnd > 0xFF)  
			|| tagType == Prod.NTAG_I2C_2k_Plus && absEnd > 0xE1)  
			throw new CommandNotSupportedException("readEEPROM is not Supported for this Phone on Second Sector");  

		byte[] temp = new byte[0];  
		answer = new byte[0];  

		if (absStart > 0xFF) {  
			absStart = 0xFF;  
		}  

		if (absEnd > 0xFF) {  
			absEnd = 0xFF;  
		}  

		int i;  
		for (i = absStart; i <= (absEnd - 3); i += 4) {  
			temp = mfu.readPages(i);  
			answer = concat(answer, temp);  
		}  

		if (i < absEnd) {  
			temp = mfu.readPages(absEnd - 3);  
			byte[] bla = Arrays.copyOfRange(temp, (i - (absEnd - 3)) * 4, 16);  
			answer = concat(answer, bla);  
		}  
		return answer;  
	}
```


- **Detail in Ntag_I2C_Command**
```java
	fetchEnd = fetchStart + maxFastRead - 1;  
	// check for last read, fetch only rest  
	if (fetchEnd > absEnd) {  
		fetchEnd = absEnd;  
	}  

	// check for sector change in between and reduce fast_read to stay within sector  
	if (getProduct() != Prod.NTAG_I2C_2k_Plus) {  
		if ((fetchStart & 0xFF00) != (fetchEnd & 0xFF00)) {  
			fetchEnd = (fetchStart & 0xFF00) + 0xFF;  
		}  
	} else {  
		if ((fetchStart & 0xFF00) == 0 && (fetchEnd > 0xE2)) {  
			fetchEnd = (fetchStart & 0xFF00) + 0xE1;  
		}  
	}  
	temp = reader.fast_read((byte) (fetchStart & 0x00FF), (byte) (fetchEnd & 0x00FF));  
	data = concat(data, temp);  

	// calculate next fetch_start  
	fetchStart = fetchEnd + 1;  

	// check for sector change in between and reduce fast_read to stay within sector  
	if (getProduct() != Prod.NTAG_I2C_2k_Plus) {  
		if ((fetchStart & 0xFF00) != (fetchEnd & 0xFF00)) {  
			reader.SectorSelect((byte) 1);  
		}  
	} else {  
		if ((fetchStart & 0xFF00) == 0 && (fetchEnd >= 0xE1)) {  
			reader.SectorSelect((byte) 1);  
			fetchStart = 0x100;  

			// Update the absEnd with pages not read on Sector 0  
			absEnd = absEnd + (0xFF - 0xE2);  
		}  
	} 
```
 

<!--stackedit_data:
eyJoaXN0b3J5IjpbNzk4NTg1NDIzXX0=
-->




##  How ReadNDEF Implements

### in Ntag_I2C_Command
#### readNDEF
> Read a NDEF Message from the tag - not an official NFC Forum NDEF detection routine.
##### describe reading process in brief
- params (none) 
- get size of **TLV** and **NDEF**
	- get **TLV** by **readEEPROM**( P1, P2 )
		- P1: Register.**User_memory_Begin**
		- P2: Register.**User_memory_Begin** + **3** 
	- read byte data using **readEEPROM**( P1, P2 )
 		- P1: Register.**User_memory_Begin**
		- P2: Register.**User_memory_Begin** + tlvPlusNdef
	-  feed byte data into **NdefMessage** as return value 
	###### about TLV (Type Length-Value)
	- A kind of data exchange format
		- T: Type information
		- L: Length information about playload data
		- V: playload data

	###### referenced in
	- Ntag_I2C_Demo
		- getRegister_Settings
		- NDEFReadTask / doInBackground
		- EEPROMSpeedtestTask / doInBackground
	###### example
	```java
		//read records of NdefMessage into String
		NdefMessage message = reader.readNDEF();  
		String NDEFText = new String(message.getRecords()[0].getPayload(),"US-ASCII");
		// Execute Read test  
		readTime = System.currentTimeMillis();  
		NdefMessage message = reader.readNDEF();  
		readTime = System.currentTimeMillis() - readTime;
	```
**Main Code**
```java
	@Override  
	public NdefMessage readNDEF() throws IOException, FormatException {  
		int ndefsize;  
		int tlvsize;  
		int tlvPlusNdef;  

		// get TLV  
		// note: [TLV] seems like a lock control  
		byte[] tlv = readEEPROM(
			Register.User_memory_Begin.getValue(),  
			Register.User_memory_Begin.getValue() + 3);  

		// checking TLV - maybe there are other TLVs on the tag  
		if (tlv[0] != 0x03) {  
			throw new FormatException("Format on Tag not supported");  
		}  
		if (tlv[1] != (byte) 0xFF) {  
			ndefsize = (tlv[1] & 0xFF);  
			tlvsize = 2;  
			tlvPlusNdef = tlvsize + ndefsize;  
		} else {  
			ndefsize = (tlv[3] & 0xFF);  
			ndefsize |= ((tlv[2] << 8) & 0xFF00);  
			tlvsize = 4;  
			tlvPlusNdef = tlvsize + ndefsize;  
		}  
		// Read NDEF Message  
		byte[] data = readEEPROM(
			Register.User_memory_Begin.getValue(),  
			Register.User_memory_Begin.getValue() + (tlvPlusNdef / 4));  

		// delete TLV  
		data = Arrays.copyOfRange(data, tlvsize, data.length);  
		// delete end of String which is not part of the NDEF Message  
		data = Arrays.copyOf(data, ndefsize);  

		// get the String out of the Message  
		NdefMessage message = new NdefMessage(data);  
		return message;
	}
```


### in MinimalNtag_I2C_Command
#### readNDEF
> Read a NDEF Message from the tag - not an official NFC Forum NDEF detection routine.
- params (none) 
- get size of **TLV** and **NDEF**
	- get **TLV** by **readEEPROM**( P1, P2 )
		- P1: Register.**User_memory_Begin**
		- P2: Register.**User_memory_Begin** + **3** 
	- read byte data using **readEEPROM**( P1, P2 )
 		- P1: Register.**User_memory_Begin**
		- P2: Register.**User_memory_Begin** + tlvPlusNdef / **4**
	-  feed byte data into **NdefMessage** as return value 
	###### about TLV (Type Length-Value)
	- A kind of data exchange format
		- T: Type information
		- L: Length information about playload data
		- V: playload data

	###### referenced in
	- Ntag_I2C_Demo
		- getRegister_Settings
		- NDEFReadTask / doInBackground
		- EEPROMSpeedtestTask / doInBackground
```java
	@Override  
	public NdefMessage readNDEF() throws IOException, FormatException, CommandNotSupportedException {  
		int ndefsize;  
		int tlvsize;  
		int tlvPlusNdef;  

		// get TLV  
		byte[] tlv = readEEPROM(
			Register.User_memory_Begin.getValue(),  
			Register.User_memory_Begin.getValue() + 3);  

		// checking TLV - maybe there are other TLVs on the tag  
		if (tlv[0] != 0x03) {  
			throw new FormatException("Format on Tag not supported");  
		}  

		if (tlv[1] != (byte) 0xFF) {  
			ndefsize = (tlv[1] & 0xFF);  
			tlvsize = 2;  
			tlvPlusNdef = tlvsize + ndefsize;  
		} else {  
			ndefsize = (tlv[3] & 0xFF);  
			ndefsize |= ((tlv[2] << 8) & 0xFF00);  
			tlvsize = 4;  
			tlvPlusNdef = tlvsize + ndefsize;  
		}  

		// Read NDEF Message  
		byte[] data = readEEPROM(
			Register.User_memory_Begin.getValue(),  
			Register.User_memory_Begin.getValue() + (tlvPlusNdef / 4));  

		// delete TLV  
		data = Arrays.copyOfRange(data, tlvsize, data.length);  
		// delete end of String which is not part of the NDEF Message  
		data = Arrays.copyOf(data, ndefsize);  

		// Interpret Bytes  
		NdefMessage message = new NdefMessage(data);  
		return message;  
	}
```
 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU5MDc2MjUwNl19
-->



## Reading Product Information

### Ntag_I2C
#### getProduct
Gets the Product of the current Tag.
#### describe implementation in brief
- params(none)
	##### referenced in
	- Ntag_I2C_Command | MinimalNtag_I2C_Command
		- writeEEPROM
		- authenticatePlus
		- protectPlus
		- unprotectPlus
	- Ntag_I2C_Command 
		-  constructor
		- getSessionRegisters
		- getConfigRegisters
		- writeConfigRegisters
		- writeEEPROM
		- readEEPROM
		- writeSRAMBlock
		- writeDeliveryNdef
	- Ntag_I2C_Demo
		- constructor
		- resetTagMemory
		- getRegister_Settings
		- readWriteConfigRegister
		- writeConfigRegisters
		- readTagContent
		- resetTagContent
		- ObtainAuthStatus
		- SRAMSpeedtestTask / doInBackground
		- EEPROMSpeedtestTask / onPreExecute
		- EEPROMSpeedtestTask / doInBackground
```java
	@Override  
	public Prod getProduct() throws IOException {  
		if (getVersionResponse == null) {  
			try {  
				getVersionResponse = new Ntag_Get_Version(reader.getVersion());  
			} catch (Exception e) {  
				e.printStackTrace();  
				try {  
					reader.close();  
					reader.connect();  
					byte[] temp = reader.read((byte) 0x00);  

					if (temp[0] == (byte) 0x04 && temp[12] == (byte) 0xE1  
					&& temp[13] == (byte) 0x10  
					&& temp[14] == (byte) 0x6D  
					&& temp[15] == (byte) 0x00) {  
						temp = reader.read((byte) 0xE8);  
						getVersionResponse = Ntag_Get_Version.NTAG_I2C_1k;  

					} else if (temp[0] == (byte) 0x04  
						&& temp[12] == (byte) 0xE1  
						&& temp[13] == (byte) 0x10  
						&& temp[14] == (byte) 0xEA  
						&& temp[15] == (byte) 0x00) {  
						getVersionResponse = Ntag_Get_Version.NTAG_I2C_2k;  
					}  
				} catch (FormatException e2) {  
					reader.close();  
					reader.connect();  
					e2.printStackTrace();  
					getVersionResponse = Ntag_Get_Version.NTAG_I2C_1k;  
				}  
			}  
		}  
		return getVersionResponse.Get_Product();  
	}
```

### MinimalNtag_I2C
product information in MinimalNtag was initialized by its constructor
```java
	@Override  
	public Prod getProduct() throws IOException {  
	  // returns generic NTAG_I2C_1k, because getVersion is not possible  
	  return tagType;  
	}
```
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDM5NzM4ODU4XX0=
-->


  
  
## Reading configRegisters/sessionRegisters/...  
  

### in Ntag_I2C_Command  

- #### Session Registers - getSessionRegister  
- #### describe writing process in brief  
 - params  
 - **SR_Offset** off  
 - offset  of the session registers  
  
 ##### referenced in  
 -  

```java  
	// referenced from   
	// 1) Ntag_I2C_Demo / .setBoardVersion()   
		-// 2) Ntag_I2C_Command / .checkPTwritePossible()  
  
```java  
	@Override    
	public byte getSessionRegister(SR_Offset off) throws IOException,FormatException{    
		byte[] register = getSessionRegisters();    
		return register[off.getValue()];    
	}  
  
	// referenced from Ntag_Demo.getSessionRegisters   
	@Override    
	public byte[] getSessionRegisters() throws IOException, FormatException {    
		if (getProduct() == Prod.NTAG_I2C_1k_Plus   
			|| getProduct() == Prod.NTAG_I2C_2k_Plus) {    
			reader.SectorSelect((byte) 0);    
			return reader.read(Register.Session_PLUS.getValue());    
		} else {    
			reader.SectorSelect((byte) 3);    
			return reader.read(Register.Session.getValue());    
		}    
	}  
```  

-   
    
  
  

#### Config Registers - getConfigRegisters  

- #### describe writing process in brief  
 - params (none)  
 ##### referenced in  
 - Ntag_I2C_Demo  
 - readWriteConfigRegister  
    

```java  
	@Override    
	public byte[] getConfigRegisters() throws IOException, FormatException {    
		/*    
		NTAG I2C plus behavior can be configured and read in two separate locations depending if the configurations shall be effective within the communication session (use session registers) or by default after Power-On Reset (POR) (use configuration registers). */    
		if (getProduct() == Prod.NTAG_I2C_1k   
		|| getProduct() == Prod.NTAG_I2C_1k_Plus  		  
		|| getProduct() == Prod.NTAG_I2C_2k_Plus)    
			reader.SectorSelect((byte) 0);    
		else if (getProduct() == Prod.NTAG_I2C_2k)    
			reader.SectorSelect((byte) 1);    
		else throw new IOException();    
  
		return reader.read(Register.Configuration.getValue());    
	}  
  
  
	// this function has not been used   
	@Override    
	public byte getConfigRegister(CR_Offset off) throws IOException,    
		FormatException {    
		byte[] register = getConfigRegisters();    
		return register[off.getValue()];    
	}  
```  

#### PT | Auth | Acess Registers  
    

```java  
	@Override    
	public byte[] getAuth0Register() throws IOException, FormatException,CommandNotSupportedException {    
		reader.SectorSelect((byte) 0);    
		return reader.read(Register.AUTH0.getValue());    
	}    
  
	@Override    
	public byte[] getAccessRegister() throws IOException, FormatException,CommandNotSupportedException {    
		reader.SectorSelect((byte) 0);    
		return reader.read(Register.ACCESS.getValue());    
	}    
  
	@Override    
	public byte[] getPTI2CRegister() throws IOException, FormatException,CommandNotSupportedException {    
		reader.SectorSelect((byte) 0);    
		return reader.read(Register.PT_I2C.getValue());    
	}  
```  
  

-------------------------------  

-------------------------------  
  

### in MinimalNtag_I2C_Command  
  

#### Session Register - getSessionRegisters  

- #### describe writing process in brief  
 - params  
 - **SR_Offset** off  
 - offset  of the session registers  
  
 ##### referenced in  
 - Ntag_I2C_Demo.setBoardVersion()   
 - Ntag_I2C_Command.checkPTwritePossible()  

------------  

### in MinimalNtag_I2C_Command  
  

- #### Config Registers  

```java  
	@Override    
	public byte[] getSessionConfigRegisters() throws IOException, FormatException,    
		CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getSessionConfigRegisters is not sSupported for this Phone with NTAG I2C 2k");    
		}    
		answer = mfu.readPages(0xEC8);    
		return answer;    
	}    
  
  
	@Override    
	public byte getSessionConfigRegister(SCR_Offset off) throws IOException,    
		FormatException, CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getSessionConfigRegister is not sSupported for this Phone with NTAG I2C 2k");    
		}    
		return getSessionConfigRegisters()[off.getValue()];    
	}  
```  
  

#### Config Registers - getConfigRegisters  

- #### describe writing process in brief  
 - params (none)  
 ##### referenced in  
 - Ntag_I2C_Demo  
 - readWriteConfig- ### Session Register  
    

```java  
	@Override    
	public byte[] getConfigSessionRegisters() throws IOException, FormatException,    
		CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getConfigSessionRegisters is not Ssupported for this Phone with NTAG I2C 2k");    
		}    
		answer = mfu.readPages(0xE8C);    
		return answer;    
	}    
  
  
	@Override    
	public byte getConfigSessionRegister(CSR_Offset off) throws IOException,    
		FormatException, CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getConfigSessionRegister is not Ssupported for this Phone with NTAG I2C 2k");    
		}    
		return getConfigSessionRegisters()[off.getValue()];    
	}  
```  

#### PT | Auth | Acess Registers  
    

```java  
	@Override    
	public byte[] getAuth0Register() throws IOException, FormatException,CommandNotSupportedException {    
		return mfu.readPages(0xE3);    
	}    
  
	@Override    
	public byte[] getAccessRegister() throws IOException, FormatException,CommandNotSupportedException {    
		return mfu.readPages(0xE4);    
	}    
  
	@Override    
	public byte[] getPTI2CRegister() throws IOException, FormatException,CommandNotSupportedException {    
		return mfu.readPages(0xE7);  
	}  
```  
  
  

---------------------  

----------  
  

### difference bewteen MinimalNtag and Ntag  

- MinimalNtag has **tagType** assertion to block unsupported product  
- MinimalNtag use **readPages(0xE8)** to fetch configuration, and use getPages(0xEC) to fetch session  
- Ntag use **read(Register.Configuration)** to fetch configuration, and use **read(Register.Session/Register.SessionPlus)** to fetch session.  
- Ntag has different sector settings between different product.  
  

--------  
  

### SR_Offset  

```java  
	public enum SR_Offset {    
		NC_REG((byte) 0x00),   
		LAST_NDEF_PAGE((byte) 0x01),  
		SM_REG((byte) 0x02),   
		WDT_LS((byte) 0x03), 		  
		WDT_MS((byte) 0x04),   
		I2C_CLOCK_STR((byte) 0x05),   
		NS_REG((byte) 0x06),   
		FIXED((byte) 0x07);     
	}  
```  

Markdown 6275  bytes 609  words 208  lines Ln 1, Col 0

HTML 4890  characters 542  words 151  paragraphs
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk3NDA5MzM2NF19
-->



##  How readSRAM implements

- ### in Ntag_I2C_Command
- #### readSRAM
- readSRAM by specifying blocks(int) and methods (RW mode)
```java
	@Override  
	public byte[] readSRAM(int blocks, R_W_Methods method) throws IOException, FormatException, TimeoutException {  
		byte[] response = new byte[0];  
		byte[] temp;  

		for (int i = 0; i < blocks; i++) {  
			if (method == R_W_Methods.Polling_Mode) {  
				waitforI2Cwrite(100);  
			} else {  
				try {  
					// else wait  
					Thread.sleep(6);  
				} catch (Exception e) {  
					e.printStackTrace();  
				}  
			} 
			temp = readSRAMBlock();  

			// concat read block to the full response  
			response = concat(response, temp);  
		}  
		answer = response;  
		return response;  
	}
```
- #### readSRAMBlock
```java
	@Override  
	public byte[] readSRAMBlock() throws IOException, FormatException {  
		answer = new byte[0];  
		if(MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
			try {  
				// authenticate using reader.pwdAuth  
				authenticatePlus(MainActivity.getPassword());  
			} catch (Exception e) {  
				e.printStackTrace();  
			}  
		}  
		reader.SectorSelect(sramSector);  
		answer = reader.fast_read((byte) 0xF0, (byte) 0xFF);  
		return answer;  
	}
```
- about sramSector
  sramSector could be 1 or zero depends on different type of nxp product
  - 1: Ntag_I2C_2k
  - 0: ...
```java
if (getProduct() == Prod.NTAG_I2C_2k) {  
  sramSector = 1;  
} else {  
  sramSector = 0;  
}
```

----------------


- ### in MinimalNtag_I2C_Command
- #### readSRAM
- readSRAM by specifying blocks(int) and methods (RW mode)
```java
	@Override  
	public byte[] readSRAM(int blocks, R_W_Methods method) throws IOException,  
		FormatException, CommandNotSupportedException {  
		if (tagType == Prod.NTAG_I2C_2k) {  
			throw new CommandNotSupportedException("readSRAM is not Supported for this Phone with NTAG I2C 2k");  
		}  
		byte[] response = new byte[0];  
		byte[] temp;  
		answer = new byte[0];  
		for (int i = 0; i < blocks; i++) {  
			if (method == R_W_Methods.Polling_Mode) {  
				waitforI2Cwrite(100);  
			} else {  
				try {  
					// else wait  
					Thread.sleep(6);  
				} catch (Exception e) {  
					e.printStackTrace();  
				}  
			} 
			temp = readSRAMBlock();  
			// concat read block to the full response  
			response = concat(response, temp);  
		}  
		answer = response;  
		return response;  
	}	 
```
- #### readSRAMBlock
```java
	@Override  
	public byte[] readSRAMBlock() throws IOException, FormatException,CommandNotSupportedException {  
		if (tagType == Prod.NTAG_I2C_2k) {  
			throw new CommandNotSupportedException("readSRAMBlock is not Supported for this Phone with NTAG I2C 2k");  
		}  
		answer = new byte[0];  
		for (int i = 0; i < 0x0F; i += 4) {  
			answer = concat(answer, mfu.readPages(0xF0 + i));  
		}  
		return answer;  
	}	 
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMzM0ODI4OF19
-->


## How MFU(MifareUltralight) / NFCA transceive Command
  
- ##### in I2C_Enabled_Commands

```java  
public static I2C_Enabled_Commands get(Tag tag) throws IOException, InterruptedException {  

...
	nfca.connect();
	command = new byte[1];  
	command[0] = (byte) 0x60; // GET_VERSION  
	answer = nfca.transceive(command);  
	prod = (new Ntag_Get_Version(answer)).Get_Product();  
	nfca.close();  

...

	nfca.connect();  
	command = new byte[2];  
	command[0] = (byte) 0xC2; //SECTOR_SELECT  
	command[1] = (byte) 0xFF;  
	answer = nfca.transceive(command);  
	nfca.close();  
 
 
	mfu.connect();  
	command = new byte[1];  
	command[0] = (byte) 0x60; // GET_VERSION  
	answer = mfu.transceive(command);  
	prod = (new Ntag_Get_Version(answer)).Get_Product();  
	mfu.close();  
	
  ...
  
	mfu.connect();  
	answer = mfu.readPages(0);  
	// no exception is thrown so the phone can use the mfu.readPages  
	// function // also check if: // - tag is from NXP (byte 0 == 0x04) // - CC corresponds to a NTAG I2C 1K  
	if (answer[0] == (byte) 0x04 && answer[12] == (byte) 0xE1  
	&& answer[13] == (byte) 0x10 && answer[14] == (byte) 0x6D  
	&& answer[15] == (byte) 0x00) {  
		// check if Config is readable (distinguish from NTAG216), if  
		// not exception is thrown, and tag is not an // NTAG I2C 1k  answer = mfu.readPages(0xE8);  
  
		// Try to read session registers to differentiate between standard and PLUS products  
		try {  
			answer = mfu.readPages(0xEC);  
			mfu.close();  
   ...
   
}  
```  

- ##### in MiniNtag_I2C_Commands

```java  
@Override  
public byte[] authenticatePlus(byte[] pwd) throws IOException, NotPlusTagException {  
	if(getProduct() != Prod.NTAG_I2C_1k_Plus && getProduct() != Prod.NTAG_I2C_2k_Plus) {  
		throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
	}  
	byte[] command = new byte[5];  
	command[0] = (byte) 0x1B;   // pwdauth
	command[1] = pwd[0];  
	command[2] = pwd[1];  
	command[3] = pwd[2];  
	command[4] = pwd[3];  
	return mfu.transceive(command);  
}
```  




## How transceive operates

- ##### SectorSelect

```java  
	command = new byte[2];  
	command[0] = (byte) 0xc2;  
	command[1] = (byte) 0xff;  
	nfca.transceive(command);  
	// pack two  
	command = new byte[4];  
	command[0] = (byte) sector;  
	command[1] = (byte) 0x00;  
	command[2] = (byte) 0x00;  
	command[3] = (byte) 0x00;  
	nfca.setTimeout(sectorSelectTimout);
	try {  
	  nfca.transceive(command);  
	} catch (IOException e) {  
	  e.printStackTrace();  
	}  
	nfca.setTimeout(timeout);  
	currentSec = sector;
```  

- ##### fast_write   

```java  
	command[0] = (byte) 0xA6;  
	command[1] = (byte) startAddr;  
	command[2] = (byte) endAddr;
	System.arraycopy(data, 0, command, 3, data.length);  
	nfca.setTimeout(500);  
	nfca.transceive(command);  
	nfca.setTimeout(timeout);
```  

- ##### write

```java  
	command = new byte[6];  
	command[0] = (byte) 0xA2;  
	command[1] = blockNr;  
	command[2] = data[0];  
	command[3] = data[1];  
	command[4] = data[2];  
	command[5] = data[3];
	nfca.transceive(command);
```  

- ##### fast_read

```java  
	command = new byte[3];  
	command[0] = (byte) 0x3A;  
	command[1] = (byte) startAddr;  
	command[2] = (byte) endAddr;
	nfca.setTimeout(500);  
	answer = nfca.transceive(command);  
	nfca.setTimeout(timeout);  
	return answer;
```  

- ##### read

```java  
	command = new byte[2];  
	command[0] = (byte) 0x30; // command:  
	command[1] = blockNr; // start page address  
	answer = nfca.transceive(command);  
	return answer;
```  


- ##### getVersion

```java  
	command = new byte[1];  
	command[0] = (byte) 0x60;  
	answer = nfca.transceive(command);  
	return answer;
```  

 - ##### pwdAuth

```java  
	command = new byte[5];  
	command[0] = (byte) 0x1B;  
	command[1] = pwd[0];  
	command[2] = pwd[1];  
	command[3] = pwd[2];  
	command[4] = pwd[3];  
	answer = nfca.transceive(command);  
	return answer;
```  





<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcxMzIxNDU5OV19
-->



## Wait for Read and Write

- ### in Ntag_I2C_Command / MinimalNtag_I2C_Command
- waitForI2CWrite
```java
	@Override  
	public void waitforI2Cwrite(int timeoutMS) throws IOException, FormatException, TimeoutException {  
		try {  
			Thread.sleep(timeoutMS);  
		} catch (InterruptedException e) {  
			e.printStackTrace();  
		}  
		return;  
	}
```

- waitForI2CRead
```java
	@Override  
	public void waitforI2Cread(int timeoutMS) throws IOException, FormatException, TimeoutException {  
		try {  
			Thread.sleep(timeoutMS);  
		} catch (InterruptedException e) {  
			e.printStackTrace();  
		}  
		return;  
	}
``` 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjgzNzU4ODldfQ==
-->



## How to Write EEPROM

### in Ntag_I2C_Command

 - **There are two different type of writeEEPROM**
	   
- writeEEPROM by providing byteData and writeListener, writeListener could be null
	   - writeEEPROM by providing byteData and startAddress
   
 - **Before writing EEPROM**
	   - ensure data length is lower than memory size
	   - sector select by **reader.SectorSelect**
	   - get block number by **Register.user_memory_Begin**
   
 - **After writting into EEPROM**
	   - check if there's onWriteListener for implementing some responses like progression bar to user.

##### referenced in
- MinimalNtag_I2C_Command | Ntag_I2C_Command
	- writeNDEF
- Ntag_I2C_Command
	- writeDeliveryNdef
- Ntag_I2C_Demo
	- resetTagContent


#### writeEEPROM (byte[] data, WriteEEPROMListener listener)
```java
@Override  
public void writeEEPROM(byte[] data, WriteEEPROMListener listener) throws IOException, FormatException {  
	if (data.length > getProduct().getMemsize()) {  
		throw new IOException("Data is to long");  
	}  
	reader.SectorSelect((byte) 0);  
	byte[] temp;  
	int index = 0;  
	byte blockNr = Register.User_memory_Begin.getValue();  

	// write till all Data is written or the Block 0xFF was written(BlockNr  
	// should be 0 then, because of the type byte)  
	for (index = 0; index < data.length && blockNr != 0; index += 4) {  
		// NTAG I2C Plus sits the Config registers in Sector 0  
		if(getProduct() == Prod.NTAG_I2C_2k_Plus && blockNr == (byte) 0xE2) {  
			break;  
		}  

		temp = Arrays.copyOfRange(data, index, index + 4);  
		reader.write(temp, blockNr);  
		blockNr++;  

		// Inform the listener about the writing  
		if(listener != null) {  
			listener.onWriteEEPROM(index + 4);  
		}  
	}  

	// If Data is left write to the 1. Sector  
	if (index < data.length) {  
		reader.SectorSelect((byte) 1);  
		blockNr = 0;  
		for (; index < data.length; index += 4) {  
			temp = Arrays.copyOfRange(data, index, index + 4);  
			reader.write(temp, blockNr);  
			blockNr++;  

			// Inform the listener about the writing  
			if(listener != null) {  
				listener.onWriteEEPROM(index + 4);  
			}  
		}  
	}  
}
```



#### writeEEPROM(int startAddr, byte[] data)
```java
	@Override  
	public void writeEEPROM(int startAddr, byte[] data) throws IOException,  
	FormatException {  
		if ((startAddr & 0x100) != 0x000 && (startAddr & 0x200) != 0x100) {  
			throw new FormatException("Sector not supported");  
		}  
		reader.SectorSelect((byte) ((startAddr & 0x200) >> 16));  
		byte[] temp;  
		int index = 0;  
		byte blockNr = (byte) (startAddr & 0xFF);  

		// write till all Data is written or the Block 0xFF was written(BlockNr  
		// should be 0 then, because of the type byte)  
		for (index = 0; index < data.length && blockNr != 0; index += 4) {  
			temp = Arrays.copyOfRange(data, index, index + 4);  
			reader.write(temp, blockNr);  
			blockNr++;  
		}  

		// If Data is left write and the first Sector was not already written  
		// switch to the first  
		if (index < data.length && (startAddr & 0x100) != 0x100) {  
			reader.SectorSelect((byte) 1);  
			blockNr = 0;  
			for (; index < data.length; index += 4) {  
				temp = Arrays.copyOfRange(data, index, index + 4);  
				reader.write(temp, blockNr);  
				blockNr++;  
			}  
		} else if ((startAddr & 0x100) == 0x100) {  
			throw new IOException("Data is to long");  
		}  
	}
```
-------------

### in MinimalNtag_I2C_Command
MinimalNtag_I2C_Command only has one implementation of writeEEPROM
#### writeEEPROM (byte[] data, WriteEEPROMListener listener)
```java
	@Override  
	public void writeEEPROM(byte[] data, WriteEEPROMListener listener) throws IOException, FormatException,  
	CommandNotSupportedException {  
		if ((tagType == Prod.NTAG_I2C_2k || tagType == Prod.NTAG_I2C_2k_Plus)  
		&& data.length > firstSectorMemsize) {  
			throw new CommandNotSupportedException("writeEEPROM is not Supported for this Phone, with Data bigger then First Sector("+ firstSectorMemsize + " Bytes)");  
		}  

		if (data.length > getProduct().getMemsize()) {  
			throw new IOException("Data is too long");  
		}  

		byte[] temp;  
		int blockNr = Register.User_memory_Begin.getValue();  

		// write till all Data is written  
		for (int i = 0; i < data.length; i += 4) {  
			temp = Arrays.copyOfRange(data, i, i + 4);  
			mfu.writePage(blockNr, temp);  
			blockNr++;  

			// Inform the listener about the writing  
			if(listener != null) {  
				listener.onWriteEEPROM(i + 4);  
			}  
		}  
	}
```

-------------

### example on writeEEPROM listener
writeEEPROM listener had been referenced in following files
- Ntag_Demo / onWriteEEPROM
- Ntag_I2C_Command / writeNDEF
- MinimalNtag_I2C_Command / writeNDEF
```java
@Override  
public void onWriteEEPROM(final int bytes) {  
	main.runOnUiThread(new Runnable() {  
		@Override  
		public void run() {  
			SpeedTestFragment.setDatarateCallback(String.valueOf(bytes) + " Bytes written");  
		}  
	});  
}

```
--------------

### Difference Between Ntag and Minimal Ntag

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgwNTUzMTgxXX0=
-->


##  How WriteNDEF Implements
#### writeNDEF
>Write a NDEF Message from the tag - not an official NFC Forum NDEF  detection routine.
#### describe writeNDEF process in brief
- **params**
	- **NdefMessage** message
		- NDEF message to write on the tag, can be created via **createNdefMessage** by a **String** text
	- **WriteEEPROMListener** listenr
		- A listener for implementing user responses, e.g. showing writing process to user
- message written into writeNDEF was created by **createNdefMessage** as a **NdefMessage**
- **NdefMessage** then to be transform into **byte[]** by **createRawNdefTlv**  (TLV seems like a lock controller)
- provide a transformed **byte[]** to be feed into **writeEEPROM**

#### about TLV (Type Length-Value)
A kind of data exchange format
T: Type information
L: Length information about playload data
V: playload data

##### referenced in
- Ntag_I2C_Demo
	- NDEF
	- doInBackground
	- NDEFWrite

### in Ntag_I2C_Command
```java
	@Override  
	public void writeNDEF(NdefMessage message, WriteEEPROMListener listener) throws IOException, FormatException {  
		byte[] Ndef_message_byte = createRawNdefTlv(message);  
		writeEEPROM(Ndef_message_byte, listener);  
	}	 
```
-----------------

### in MinimalNtag_I2C_Command
```java
	@Override  
	public void writeNDEF(NdefMessage message, WriteEEPROMListener listener) throws IOException, FormatException, CommandNotSupportedException {  
		byte[] ndefMessageByte = createRawNdefTlv(message);  
		writeEEPROM(ndefMessageByte, listener);  
	}
```
-------------

###  writeEEPROM listener
writeEEPROM listener has been referenced in following files
- Ntag_Demo / onWriteEEPROM
- Ntag_I2C_Command / writeNDEF
- MinimalNtag_I2C_Command / writeNDEF

```java
@Override  
public void onWriteEEPROM(final int bytes) {  
	main.runOnUiThread(new Runnable() {  
		@Override  
		public void run() {  
			SpeedTestFragment.setDatarateCallback(String.valueOf(bytes) + " Bytes written");  
		}  
	});  
}
```
---------------------

### CreateNDEFMessage
##### referenced in
- Ntag_Demo
	- onPreExecute
	- 
```java
	private NdefMessage createNdefMessage(String text) throws UnsupportedEncodingException {  
		String lang = "en";  
		byte[] textBytes = text.getBytes();  
		byte[] langBytes = lang.getBytes("US-ASCII");  
		int langLength = langBytes.length;  
		int textLength = textBytes.length;  
		byte[] payload = new byte[1 + langLength + textLength];  
		payload[0] = (byte) langLength;  
		System.arraycopy(langBytes, 0, payload, 1, langLength);  
		System.arraycopy(textBytes, 0, payload, 1 + langLength, textLength);  

		NdefRecord record = new NdefRecord(NdefRecord.TNF_WELL_KNOWN,  
		NdefRecord.RTD_TEXT, new byte[0], payload);  
		NdefRecord[] records = { record };  
		NdefMessage message = new NdefMessage(records);  
		return message;  
	}  
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk0NTIyMTgwXX0=
-->

## How Write Auth | Config Registers Imlements

### Write Auth Registers
#### Ntag_I2C_Command
##### writeAuthRegisters
##### describe writing process in brief
- params
	- **byte** auth0
	- **byte** access
	- **byte** ptI2C
- write following configurations by reader.**write**
	- write access configuration  
	- write PT(Pass Thru) configuration 
	- write password 
	- write auth0  

###### referenced in
- Ntag_I2C_Demo
	- resetTagMemory
	- writeConfigRegisters

```java
	@Override  
	public void writeAuthRegisters(byte auth0, byte access, byte ptI2C) throws IOException, FormatException, CommandNotSupportedException {  
	  byte[] data = new byte[4];  
	  
	  reader.SectorSelect((byte) 0);  
	  
	  // Write the ACCESS configuration  
	  data[0] = access;  
	  data[1] = 0x00;  
	  data[2] = 0x00;  
	  data[3] = 0x00;  
	  reader.write(data, Register.ACCESS.getValue());  
	  
	  // Write the PT I2C configuration  
	  data[0] = ptI2C;  
	  data[1] = 0x00;  
	  data[2] = 0x00;  
	  data[3] = 0x00;  
	  reader.write(data, Register.PT_I2C.getValue());  
	  
	  // Set the password to FFs  
	  data[0] = (byte) 0xFF;  
	  data[1] = (byte) 0xFF;  
	  data[2] = (byte) 0xFF;  
	  data[3] = (byte) 0xFF;  
	  reader.write(data, Register.PWD.getValue());  
	  
	  // Set the pack to 00s  
	  data[0] = (byte) 0x00;  
	  data[1] = (byte) 0x00;  
	  data[2] = (byte) 0x00;  
	  data[3] = (byte) 0x00;  
	  reader.write(data, Register.PACK.getValue());  
	  
	  data[0] = 0x00;  
	  data[1] = 0x00;  
	  data[2] = 0x00;  
	  data[3] = auth0;  
	  reader.write(data, Register.AUTH0.getValue());  
	}
```

#### MinimalNtag_I2C_Command

##### writeAuthRegisters
##### describe writing process in brief
- params
	- **byte** auth0
	- **byte** access
	- **byte** ptI2C
- write following configurations by mfu.**writePage**
	- write access configuration  
	- write PT(Pass Thru) configuration 
	- write password 
	- write auth0  

###### referenced in
- Ntag_I2C_Demo
	- resetTagMemory
	- writeConfigRegisters
	
```java
	@Override  
	public void writeAuthRegisters(byte auth0, byte access, byte ptI2C) throws IOException, FormatException,CommandNotSupportedException {  
	  byte[] data = new byte[4];  
	  
	  // Write the ACCESS configuration  
	  data[0] = access;  
	  data[1] = 0x00;  
	  data[2] = 0x00;  
	  data[3] = 0x00;  
	  mfu.writePage(0xE4, data);  
	  
	  // Write the PT I2C configuration  
	  data[0] = ptI2C;  
	  data[1] = 0x00;  
	  data[2] = 0x00;  
	  data[3] = 0x00;  
	  mfu.writePage(0xE7, data);  
	  
	  // Set the password to FFs  
	  data[0] = (byte) 0xFF;  
	  data[1] = (byte) 0xFF;  
	  data[2] = (byte) 0xFF;  
	  data[3] = (byte) 0xFF;  
	  mfu.writePage(0xE5, data);  
	  
	  // Set the pack to 00s  
	  data[0] = (byte) 0x00;  
	  data[1] = (byte) 0x00;  
	  data[2] = (byte) 0x00;  
	  data[3] = (byte) 0x00;  
	  mfu.writePage(0xE6, data);  
	  
	  data[0] = 0x00;  
	  data[1] = 0x00;  
	  data[2] = 0x00;  
	  data[3] = auth0;  
	  mfu.writePage(0xE3, data);  
	}
```

### Write Config Registers

#### Ntag_I2C_Command
##### writeConfigRegisters
###### referenced in
- Ntag_I2C_Demo
	- resetTagMemory
	- writeConfigRegisters
```java
	@Override  
	public void writeConfigRegisters(byte NC_R, byte LD_R, byte SM_R, byte WD_LS_R, byte WD_MS_R, byte I2C_CLOCK_STR) throws IOException,FormatException {  
		byte[] Data = new byte[4];  
		if (getProduct() == Prod.NTAG_I2C_1k 
		|| getProduct() == Prod.NTAG_I2C_1k_Plus 
		|| getProduct() == Prod.NTAG_I2C_2k_Plus)  
			reader.SectorSelect((byte) 0);  
		else if (getProduct() == Prod.NTAG_I2C_2k)  
			reader.SectorSelect((byte) 1);  
		else throw new IOException();  
		
		// Write the Config Regs  
		Data[0] = NC_R;  
		Data[1] = LD_R;  
		Data[2] = SM_R;  
		Data[3] = WD_LS_R;  
		reader.write(Data, Register.Configuration.getValue());  

		Data[0] = WD_MS_R;  
		Data[1] = I2C_CLOCK_STR;  
		Data[2] = (byte) 0x00;  
		Data[3] = (byte) 0x00;  
		reader.write(Data, (byte) (Register.Configuration.getValue() + 1));  
	}
```

#### MinimalNtag_I2C_Command
##### writeConfigRegisters
###### referenced in
- Ntag_I2C_Demo
	- resetTagMemory
	- writeConfigRegisters
```java
	@Override  
	public void writeConfigRegisters(byte ncR, byte ldR, byte smR, byte wdLsR, byte wdMsR, byte i2CClockStr) throws IOException, FormatException, CommandNotSupportedException {  
		if (tagType == Prod.NTAG_I2C_2k) {  
			throw new CommandNotSupportedException("writeConfigRegisters is not Supported for this Phone with NTAG I2C 2k");  
		}  
		byte[] data = new byte[4];  

		// Write the Config Regs  
		data[0] = ncR;  
		data[1] = ldR;  
		data[2] = smR;  
		data[3] = wdLsR;  
		mfu.writePage(0xE8, data);  

		data[0] = wdMsR;  
		data[1] = i2CClockStr;  
		data[2] = (byte) 0x00;  
		data[3] = (byte) 0x00;  
		mfu.writePage(0xE9, data);  
	}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MDE4OTQ2NDNdfQ==
-->


##  How Write SRAM Implements


### in Ntag_I2C_Command
#### writeSRAM
##### describe writing process in brief
- **params**
	- **byte[]** data
	- **WriteSRAMListener** listenr
		- A listener for implementing user responses, e.g. showing writing process to user
- Calculate data blocks by dividing data into dataBlocks of each 64 bytes
- Write each dataBlock by **writeSRAMBlock**
- Check if Ntag is in **Polling_Mode** to determine whether needs to wait for device or not

	###### About Polling Mode
	Polling is the process where the computer or controlling device waits for an [external device](https://en.wikipedia.org/wiki/External_device "External device") to check for its readiness or state, often with low-level hardware.

	###### referenced in
	- Ntag_Demo / Flash

```java
	@Override  
	public void writeSRAM(byte[] data, R_W_Methods method, WriteSRAMListener listener) throws IOException, FormatException, TimeoutException {  
		int blocks = (int) Math.ceil(data.length / 64.0);  
		for (int i = 0; i < blocks; i++) {  
			byte[] dataBlock = new byte[64];  
			if (data.length - (i + 1) * 64 < 0) {  
				Arrays.fill(dataBlock, (byte) 0);  
				System.arraycopy(data, i * 64, dataBlock, 0, data.length % 64);  
			} else {  
				System.arraycopy(data, i * 64, dataBlock, 0, 64);  
			}  
			writeSRAMBlock(dataBlock, listener);  
			if (method == R_W_Methods.Polling_Mode) {  
				waitforI2Cread(100);  
			} else {  
				try {  
				// else wait  
				Thread.sleep(6);  
				} catch (Exception e) {  
					e.printStackTrace();  
				}  
			}  
		}  
	}   
```

#### writeSRAMBlock
##### describe writing process in brief
- **params**
	- **byte[]** data
	- **WriteSRAMListener** listener
		- A listener for implementing user responses, e.g. inform  to user about the writing process
- get **sramSector**
	- about sramSector
  sramSector could be 1 or zero depends on different type of nxp product (1) Ntag_I2C_2k (0) ...
- determine if it can be applied on fast_write mode
	- only NTAG_I2C_X_Plus has fast_write/ fast_read mode
	- in fast_write, data is directly written in SRAM
	- only non-authenticated state in AuthState can be applied into fast_write 
- if not, use MifareUltralight 

	###### referenced in
		- Ntag_I2C_Demo / setBoardVersion
		- Ntag_I2C_Demo / doInBackground
		- Ntag_I2C_Command / writeSRAM
 		- MinimalNtag_I2C_Command / writeSRAM
 		- 
```java
	@Override  
	public void writeSRAMBlock(byte[] data, WriteSRAMListener listener) throws IOException,FormatException {  
		byte[] txBuffer = new byte[4];  
		int index = 0;  
		reader.SectorSelect(sramSector);  
		/*
		* Samsung controllers do not like NfcA Transceive method, 
		* so it is better using MUL writePage command when possible 
		* For NTAG_I2C_2k it is not possible to use MUL commands because when 
		* establishing the connection the Sector is moved back to 0 */  
		if((getProduct() == Prod.NTAG_I2C_1k_Plus 
			|| getProduct() == Prod.NTAG_I2C_2k_Plus)  
			&& MainActivity.getAuthStatus() != AuthStatus.Authenticated.getValue()) {  
			reader.fast_write(
				data, 
				(byte) Register.SRAM_Begin.getValue(),  
				(byte) (Register.SRAM_Begin.getValue() + 0x0F));  
		} else {  
			if (getProduct() == Prod.NTAG_I2C_1k) {  
				reader.close();  
				MifareUltralight ul = MifareUltralight.get(tag);  
				if (ul != null) {  
					ul.connect();  
					int SRAM_Begin = (int) Register.SRAM_Begin.getValue() & 0xFF;  
					for (int i = 0; i < 16; i++) {  
						for (int dI = 0; dI < 4; dI++) {  
							if (index < data.length) {  
								txBuffer[dI] = data[index++];  
							} else {  
								txBuffer[dI] = (byte) 0x00;  
							}  
						}  
						ul.writePage(SRAM_Begin + i, txBuffer);  
					}  
					ul.close();  
				}  
				reader.connect();  
			} else {  
				if(MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
					try {  
						// NTAG_I2C_1k_Plus | NTAG_I2C_2k_Plus
						authenticatePlus(MainActivity.getPassword());  
					} catch (Exception e) {  
						e.printStackTrace();  
					}  
				}  
				// if not authenticated and not one of (NTAG_I2C_1k_Plus | NTAG_I2C_2k_Plus)
				for (int i = 0; i < 16; i++) {  
					for (int dI = 0; dI < 4; dI++) {  
						if (index < data.length) {  
							txBuffer[dI] = data[index++];  
						} else {  
							txBuffer[dI] = (byte) 0x00;  
						}  
					}  
					reader.write(txBuffer, (byte) (Register.SRAM_Begin.getValue() + i));  
				}  
			}  
		}  
		// Inform the listener about the writing  
		if(listener != null) {  
			listener.onWriteSRAM();  
		}  
	}
```
-----------------
-----------------


### in MinimalNtag_I2C_Command

#### writeSRAM
##### describe writing process in brief
- **params**
	- **byte[]** data
	- **WriteSRAMListener** listenr
		- A listener for implementing user responses, e.g. showing writing process to user
- MinimalNtag not support for NTAG_I2C_2k
- Calculate data blocks by dividing data into dataBlocks of each 64 bytes
- Write each dataBlock by **writeSRAMBlock**
- Check if Ntag is in **Polling_Mode** to determine whether needs to wait for device or not

	###### referenced in
	- Ntag_I2C_Demo / setBoardVersion
	- Ntag_I2C_Demo / doInBackground
	- Ntag_I2C_Command / writeSRAM
	- MinimalNtag_I2C_Command / writeSRAM

```java
	@Override  
	public void writeSRAM(byte[] data, R_W_Methods method, WriteSRAMListener listener) throws IOException, FormatException, CommandNotSupportedException {  
		if (tagType == Prod.NTAG_I2C_2k) {  
			throw new CommandNotSupportedException("writeSRAM is not Supported for this Phone with NTAG I2C 2k");  
		}  
		int blocks = (int) Math.ceil(data.length / 64.0);  
		for (int i = 0; i < blocks; i++) {  
			writeSRAMBlock(data, listener);  
			if (method == R_W_Methods.Polling_Mode) {  
				waitforI2Cread(100);  
			} else {  
				try {  
					// else wait  
					Thread.sleep(6);  
				} catch (Exception e) {  
					e.printStackTrace();  
				}  
			}  
			if (data.length > 64) {  
				data = Arrays.copyOfRange(data, 64, data.length);  
			}  
		}  
	}
```



#### writeSRAMBlock
##### describe writing process in brief
- **params**
	- **byte[]** data
	- **WriteSRAMListener** listener
		- A listener for implementing user responses, e.g. inform  to user about the writing process
- MinimalNtag not support for NTAG_I2C_2k
- mfu.**writePage**


	###### referenced in
	- Ntag_Demo / Flash


```java
	@Override  
	public void writeSRAMBlock(byte[] data, WriteSRAMListener listener) throws IOException,  
		FormatException, CommandNotSupportedException {  
		if (tagType == Prod.NTAG_I2C_2k) {  
			throw new CommandNotSupportedException("writeSRAMBlock is not Supported for this Phone with NTAG I2C 2k");  
		}  
		byte[] txBuffer = new byte[4];  
		int index = 0;  

		for (int i = 0; i < 16; i++) {  
			for (int dI = 0; dI < 4; dI++) {  
				if (index < data.length) {  
					txBuffer[dI] = data[index++];  
				} else {  
					txBuffer[dI] = (byte) 0x00;  
				}  
			}  
			mfu.writePage(0xF0 + i, txBuffer);  
		}  
		// Inform the listener about the writing  
		if(listener != null) {  
			listener.onWriteSRAM();  
		}  
	}
```

-------------

####  writeSRAM listener
WriteSRAMListener has been referenced in following files
- Ntag_Demo / onWriteSRAM
- Ntag_I2C_Command / writeSRAMBlock
- MinimalNtag_I2C_Command / writeSRAMBlock

```java
	@Override  
	public void onWriteSRAM() {  
		main.runOnUiThread(new Runnable() {  
			@Override  
			public void run() {  
				FlashMemoryActivity.updateFLashDialog();  
			}  
		});  
	}
```
---------------------

#### R_W_Methods
there are three R/W modes in Ntag
- fast_mode
- polling_mode
- error mode
```java
	public enum R_W_Methods {  
	   Fast_Mode, Polling_Mode, Error  
	} ###### ReadMemoryActivity 
``` 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzc4NDkyNzg2XX0=
-->
