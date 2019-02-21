



### NDEF()
read | write NDEF
``` mermaid
graph LR
 
$Write
!Write
$addAar

subgraph NdefFragment
	NdefText
	NdefUri
	NdefBT
	NdefSp
	ndefType
end

msg --> ndefType
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

msg --> aarRecord
msg -.- NDEFWrite
msg0 -.- NDEFWrite

```
msg: **NDEFMessage** | write: **NdefFragment.Boolean**

```kotlin
public void NDEF() throws IOException {
		// Check if the operation is read or write
		if (NdefFragment.isWriteChosen()) {
			// NDEF Message to write in the tag
			NdefMessage msg = null;

			// Get the selected NDEF type since the creation of the NDEF Msg
			// will vary depending on the type
			if (NdefFragment.getNdefType().equalsIgnoreCase(
					main.getResources().getString(R.string.radio_text))) {
				msg = createNdefTextMessage(NdefFragment.getText());
			} else if (NdefFragment.getNdefType().equalsIgnoreCase(
					main.getResources().getString(R.string.radio_uri))) {
				msg = createNdefUriMessage(NdefFragment.getText());
			} else if (NdefFragment.getNdefType().equalsIgnoreCase(
					main.getResources().getString(R.string.radio_btpair))) {
				msg = createNdefBSSPMessage();
			} else if (NdefFragment.getNdefType().equalsIgnoreCase(
					main.getResources().getString(R.string.radio_sp))) {
				msg = createNdefSpMessage(NdefFragment.getSpTitle(), NdefFragment.getSpLink());
			}
			if(msg == null) {
				toastText(main, "Please add correct input values", Toast.LENGTH_LONG) ;
				NdefFragment.setAnswer(main.getResources().getString(R.string.format_error));
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA3OTkwMDgxMiwtODI4NTU2MzNdfQ==
-->