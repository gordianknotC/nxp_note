


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