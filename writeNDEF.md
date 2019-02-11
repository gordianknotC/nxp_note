

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