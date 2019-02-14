

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