

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