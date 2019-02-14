
#  How Write SRAM Implements


## in Ntag_I2C_Command
### writeSRAM
#### describe writing process in brief
- **params**
	- **byte[]** data
	- **WriteSRAMListener** listenr
		- A listener for implementing user responses, e.g. showing writing process to user
- Calculate data blocks by dividing data into dataBlocks of each 64 bytes
- Write each dataBlock by **writeSRAMBlock**
- Check if Ntag is in **Polling_Mode** to determine whether needs to wait for device or not

	##### About Polling Mode
	Polling is the process where the computer or controlling device waits for an [external device](https://en.wikipedia.org/wiki/External_device "External device") to check for its readiness or state, often with low-level hardware.

	##### referenced in
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

### writeSRAMBlock
#### describe writing process in brief
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

	##### referenced in
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


## in MinimalNtag_I2C_Command

### writeSRAM
#### describe writing process in brief
- **params**
	- **byte[]** data
	- **WriteSRAMListener** listenr
		- A listener for implementing user responses, e.g. showing writing process to user
- MinimalNtag not support for NTAG_I2C_2k
- Calculate data blocks by dividing data into dataBlocks of each 64 bytes
- Write each dataBlock by **writeSRAMBlock**
- Check if Ntag is in **Polling_Mode** to determine whether needs to wait for device or not

	##### referenced in
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



### writeSRAMBlock
#### describe writing process in brief
- **params**
	- **byte[]** data
	- **WriteSRAMListener** listener
		- A listener for implementing user responses, e.g. inform  to user about the writing process
- MinimalNtag not support for NTAG_I2C_2k
- mfu.**writePage**


	##### referenced in
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

###  writeSRAM listener
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

### R_W_Methods
there are three R/W modes in Ntag
- fast_mode
- polling_mode
- error mode
```java
	public enum R_W_Methods {  
	   Fast_Mode, Polling_Mode, Error  
	} ##### ReadMemoryActivity 
``` 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzc4NDkyNzg2XX0=
-->