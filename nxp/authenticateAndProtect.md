



# Authenticate And Protect

## Authenticate
### Ntag_I2C_Command
#### authenticatePlus
Authenticate using PWD_AUTH command against NTAG I2C Plus product.

**describe writing process in brief**
- params
	- **byte[]** pwd :: 4Byte password to authenticate with
- check if its an **Ntag_I2C_Plus** product since only **Plus** support auth operation.
- read auth data by using reader.**pwdAuth**

**referenced in**
- Ntag_I2C_Demo
	- constructor
	- Auth
	- SRAMSpeedtestTask / doInBackground
	- EEPROMSpeedtestTask / doInBackground
- Ntag_I2C_Command  
	- writeSRAMBlock
	- readSRAMBlock
```java
@Override  
public byte[] authenticatePlus(byte[] pwd) throws IOException, NotPlusTagException {  
  if(getProduct() != Prod.NTAG_I2C_1k_Plus && getProduct() != Prod.NTAG_I2C_2k_Plus) {  
    throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
  }  
  return reader.pwdAuth(pwd);  
}
```
### MinimalNtag_I2C_Command
#### authenticatePlus
Authenticate using PWD_AUTH command against NTAG I2C Plus product.
- #### describe writing process in brief
	- params
		- **byte[]** pwd
			- 4Byte password to authenticate with
	- check if its an **Ntag_I2C_Plus** product since only **Plus** support auth operation.
	- perform auth operation by using **mfu.transceive**
		##### referenced in
		- Ntag_I2C_Demo
			- constructor
			- Auth
			- SRAMSpeedtestTask / doInBackground
			- EEPROMSpeedtestTask / doInBackground
		
```java
@Override  
public byte[] authenticatePlus(byte[] pwd) throws IOException, NotPlusTagException {  
	if(getProduct() != Prod.NTAG_I2C_1k_Plus && getProduct() != Prod.NTAG_I2C_2k_Plus) {  
		throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
	}  
	byte[] command = new byte[5];  
	command[0] = (byte) 0x1B;  
	command[1] = pwd[0];  
	command[2] = pwd[1];  
	command[3] = pwd[2];  
	command[4] = pwd[3];  
	return mfu.transceive(command);  
}
```

## Protect
### Ntag_I2C_Command
#### protectPlus
Protect NTAG I2C Plus product memory map.
- #### describe writing process in brief
	- params
		- **byte[]** pwd
			- 4Byte password to authenticate with
		- **byte** staretAddr
			- Page to lock the memory from
	- check if it's an **Ntag_I2C_Plus** product, since auth operation not supported by non Ntag_I2C_Plus 
	- reader.**SectorSelect** 0
	- set password indicated by user - using reader.**write**(pwd, Register.PWD)
	- write access configuration
	- write pt (pass thru) configuration
	- write auth0 configuration
	
		##### referenced in
		- Ntag_I2C_Demo
			- Auth
```java
	@Override  
	public void protectPlus(byte[] pwd, byte startAddr) throws IOException, FormatException, NotPlusTagException {  
		byte[] data = new byte[4];  

		if(getProduct() != Prod.NTAG_I2C_1k_Plus 
		&& getProduct() != Prod.NTAG_I2C_2k_Plus) {  
			throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
		}  
		reader.SectorSelect((byte) 0);  

		// Set the password indicated by the user  
		reader.write(pwd, Register.PWD.getValue());  

		byte access = (byte) 0x00;  
		byte authLimit = 0x00; // Don't limit the number of auth attempts  

		access ^= 1 << Access_Offset.NFC_PROT.getValue(); // NFC_Prot  
		access ^= 0 << Access_Offset.NFC_DIS_SEC1.getValue(); // NFC_DIS_SEC1  
		access |= authLimit << Access_Offset.AUTH_LIM.getValue(); // AUTHLIM  

		// Write the ACCESS configuration  data[0] = access;  
		data[1] = 0x00;  
		data[2] = 0x00;  
		data[3] = 0x00;  
		reader.write(data, Register.ACCESS.getValue());  

		byte ptI2C = 0x00;  
		byte i2CProt = 0x00;  

		ptI2C ^= 0 << PT_I2C_Offset.K2_PROT.getValue(); // 2K Prot  
		ptI2C ^= 1 << PT_I2C_Offset.SRAM_PROT.getValue(); // SRAM Prot  
		ptI2C |= i2CProt << PT_I2C_Offset.I2C_PROT.getValue(); // I2C Prot  

		// Write the PT_I2C configuration  data[0] = ptI2C;  
		data[1] = 0x00;  
		data[2] = 0x00;  
		data[3] = 0x00;  
		reader.write(data, Register.PT_I2C.getValue());  

		// Write the AUTH0 lock starting page  
		data[0] = 0x00;  
		data[1] = 0x00;  
		data[2] = 0x00;  
		data[3] = startAddr;  
		reader.write(data, Register.AUTH0.getValue());  
	}
```

### MinimalNtag_I2C_Command
almost the same as Ntag_I2C, except using mfu.writePage


## Unprotect
behaviors almost the same as method of protect referred above.

### Ntag_I2C_Command
### MinimalNtag_I2C_Command
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjMxNzQ2MDk4XX0=
-->