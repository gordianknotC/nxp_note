

## How MFU(MifareUltralight) / NFCA transceive Command
  
- ##### in I2C_Enabled_Commands

```java  
public static I2C_Enabled_Commands get(Tag tag) throws IOException, InterruptedException {  

...
	nfca.connect();
	command = new byte[1];  
	command[0] = (byte) 0x60; // GET_VERSION  
	answer = nfca.transceive(command);  
	prod = (new Ntag_Get_Version(answer)).Get_Product();  
	nfca.close();  

...

	nfca.connect();  
	command = new byte[2];  
	command[0] = (byte) 0xC2; //SECTOR_SELECT  
	command[1] = (byte) 0xFF;  
	answer = nfca.transceive(command);  
	nfca.close();  
 
 
	mfu.connect();  
	command = new byte[1];  
	command[0] = (byte) 0x60; // GET_VERSION  
	answer = mfu.transceive(command);  
	prod = (new Ntag_Get_Version(answer)).Get_Product();  
	mfu.close();  
	
  ...
  
	mfu.connect();  
	answer = mfu.readPages(0);  
	// no exception is thrown so the phone can use the mfu.readPages  
	// function // also check if: // - tag is from NXP (byte 0 == 0x04) // - CC corresponds to a NTAG I2C 1K  
	if (answer[0] == (byte) 0x04 && answer[12] == (byte) 0xE1  
	&& answer[13] == (byte) 0x10 && answer[14] == (byte) 0x6D  
	&& answer[15] == (byte) 0x00) {  
		// check if Config is readable (distinguish from NTAG216), if  
		// not exception is thrown, and tag is not an // NTAG I2C 1k  answer = mfu.readPages(0xE8);  
  
		// Try to read session registers to differentiate between standard and PLUS products  
		try {  
			answer = mfu.readPages(0xEC);  
			mfu.close();  
   ...
   
}  
```  

- ##### in MiniNtag_I2C_Commands

```java  
@Override  
public byte[] authenticatePlus(byte[] pwd) throws IOException, NotPlusTagException {  
	if(getProduct() != Prod.NTAG_I2C_1k_Plus && getProduct() != Prod.NTAG_I2C_2k_Plus) {  
		throw new NotPlusTagException("Auth Operations are not supported by non NTAG I2C PLUS products");  
	}  
	byte[] command = new byte[5];  
	command[0] = (byte) 0x1B;   // pwdauth
	command[1] = pwd[0];  
	command[2] = pwd[1];  
	command[3] = pwd[2];  
	command[4] = pwd[3];  
	return mfu.transceive(command);  
}
```  




## How transceive operates

- ##### SectorSelect

```java  
	command = new byte[2];  
	command[0] = (byte) 0xc2;  
	command[1] = (byte) 0xff;  
	nfca.transceive(command);  
	// pack two  
	command = new byte[4];  
	command[0] = (byte) sector;  
	command[1] = (byte) 0x00;  
	command[2] = (byte) 0x00;  
	command[3] = (byte) 0x00;  
	nfca.setTimeout(sectorSelectTimout);
	try {  
	  nfca.transceive(command);  
	} catch (IOException e) {  
	  e.printStackTrace();  
	}  
	nfca.setTimeout(timeout);  
	currentSec = sector;
```  

- ##### fast_write   

```java  
	command[0] = (byte) 0xA6;  
	command[1] = (byte) startAddr;  
	command[2] = (byte) endAddr;
	System.arraycopy(data, 0, command, 3, data.length);  
	nfca.setTimeout(500);  
	nfca.transceive(command);  
	nfca.setTimeout(timeout);
```  

- ##### write

```java  
	command = new byte[6];  
	command[0] = (byte) 0xA2;  
	command[1] = blockNr;  
	command[2] = data[0];  
	command[3] = data[1];  
	command[4] = data[2];  
	command[5] = data[3];
	nfca.transceive(command);
```  

- ##### fast_read

```java  
	command = new byte[3];  
	command[0] = (byte) 0x3A;  
	command[1] = (byte) startAddr;  
	command[2] = (byte) endAddr;
	nfca.setTimeout(500);  
	answer = nfca.transceive(command);  
	nfca.setTimeout(timeout);  
	return answer;
```  

- ##### read

```java  
	command = new byte[2];  
	command[0] = (byte) 0x30; // command:  
	command[1] = blockNr; // start page address  
	answer = nfca.transceive(command);  
	return answer;
```  


- ##### getVersion

```java  
	command = new byte[1];  
	command[0] = (byte) 0x60;  
	answer = nfca.transceive(command);  
	return answer;
```  

 - ##### pwdAuth

```java  
	command = new byte[5];  
	command[0] = (byte) 0x1B;  
	command[1] = pwd[0];  
	command[2] = pwd[1];  
	command[3] = pwd[2];  
	command[4] = pwd[3];  
	answer = nfca.transceive(command);  
	return answer;
```  





<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcxMzIxNDU5OV19
-->