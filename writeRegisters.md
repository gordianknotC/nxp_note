
# How Write Auth | Config Registers Imlements

## Write Auth Registers
### Ntag_I2C_Command
#### writeAuthRegisters
#### describe writing process in brief
- params
	- **byte** auth0
	- **byte** access
	- **byte** ptI2C
- write following configurations by reader.**write**
	- write access configuration  
	- write PT(Pass Thru) configuration 
	- write password 
	- write auth0  

##### referenced in
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

### MinimalNtag_I2C_Command

#### writeAuthRegisters
#### describe writing process in brief
- params
	- **byte** auth0
	- **byte** access
	- **byte** ptI2C
- write following configurations by mfu.**writePage**
	- write access configuration  
	- write PT(Pass Thru) configuration 
	- write password 
	- write auth0  

##### referenced in
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

## Write Config Registers

### Ntag_I2C_Command
#### writeConfigRegisters
##### referenced in
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

### MinimalNtag_I2C_Command
#### writeConfigRegisters
##### referenced in
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