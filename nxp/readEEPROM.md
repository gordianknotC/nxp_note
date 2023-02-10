



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
