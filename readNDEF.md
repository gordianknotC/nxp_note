


#  How ReadNDEF Implements

## in Ntag_I2C_Command
### readNDEF
> Read a NDEF Message from the tag - not an official NFC Forum NDEF detection routine.
#### describe reading process in brief
- params (none) 
- get size of **TLV** and **NDEF**
	- get **TLV** by **readEEPROM**( P1, P2 )
		- P1: Register.**User_memory_Begin**
		- P2: Register.**User_memory_Begin** + **3** 
	- read byte data using **readEEPROM**( P1, P2 )
 		- P1: Register.**User_memory_Begin**
		- P2: Register.**User_memory_Begin** + tlvPlusNdef
	-  feed byte data into **NdefMessage** as return value 
	##### about TLV (Type Length-Value)
	- A kind of data exchange format
		- T: Type information
		- L: Length information about playload data
		- V: playload data

	##### referenced in
	- Ntag_I2C_Demo
		- getRegister_Settings
		- NDEFReadTask / doInBackground
		- EEPROMSpeedtestTask / doInBackground
	##### example
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


## in MinimalNtag_I2C_Command
### readNDEF
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
	##### about TLV (Type Length-Value)
	- A kind of data exchange format
		- T: Type information
		- L: Length information about playload data
		- V: playload data

	##### referenced in
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