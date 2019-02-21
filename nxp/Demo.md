



### NDEF()
read | write NDEF
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

NdefText --> createNdefTextMessage
NdefUri --> createNdefUriMessage
NdefBT --> createNdefBSSPMessage
NdefSp --> createNdefSpMessage
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


### NDEFRead
```mermaid
graph LR
timeStart
timeEnd
readTime

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ3MDM0OTIwNywxNzU5NTY1OTA1LDIxNz
E1ODk5NCwxOTg1MzkxMTc4LC04Mjg1NTYzM119
-->