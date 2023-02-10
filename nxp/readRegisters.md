

  
  
## Reading configRegisters/sessionRegisters/...  
  

### in Ntag_I2C_Command  

- #### Session Registers - getSessionRegister  
- #### describe writing process in brief  
 - params  
 - **SR_Offset** off  
 - offset  of the session registers  
  
 ##### referenced in  
 -  

```java  
	// referenced from   
	// 1) Ntag_I2C_Demo / .setBoardVersion()   
		-// 2) Ntag_I2C_Command / .checkPTwritePossible()  
  
```java  
	@Override    
	public byte getSessionRegister(SR_Offset off) throws IOException,FormatException{    
		byte[] register = getSessionRegisters();    
		return register[off.getValue()];    
	}  
  
	// referenced from Ntag_Demo.getSessionRegisters   
	@Override    
	public byte[] getSessionRegisters() throws IOException, FormatException {    
		if (getProduct() == Prod.NTAG_I2C_1k_Plus   
			|| getProduct() == Prod.NTAG_I2C_2k_Plus) {    
			reader.SectorSelect((byte) 0);    
			return reader.read(Register.Session_PLUS.getValue());    
		} else {    
			reader.SectorSelect((byte) 3);    
			return reader.read(Register.Session.getValue());    
		}    
	}  
```  

-   
    
  
  

#### Config Registers - getConfigRegisters  

- #### describe writing process in brief  
 - params (none)  
 ##### referenced in  
 - Ntag_I2C_Demo  
 - readWriteConfigRegister  
    

```java  
	@Override    
	public byte[] getConfigRegisters() throws IOException, FormatException {    
		/*    
		NTAG I2C plus behavior can be configured and read in two separate locations depending if the configurations shall be effective within the communication session (use session registers) or by default after Power-On Reset (POR) (use configuration registers). */    
		if (getProduct() == Prod.NTAG_I2C_1k   
		|| getProduct() == Prod.NTAG_I2C_1k_Plus  		  
		|| getProduct() == Prod.NTAG_I2C_2k_Plus)    
			reader.SectorSelect((byte) 0);    
		else if (getProduct() == Prod.NTAG_I2C_2k)    
			reader.SectorSelect((byte) 1);    
		else throw new IOException();    
  
		return reader.read(Register.Configuration.getValue());    
	}  
  
  
	// this function has not been used   
	@Override    
	public byte getConfigRegister(CR_Offset off) throws IOException,    
		FormatException {    
		byte[] register = getConfigRegisters();    
		return register[off.getValue()];    
	}  
```  

#### PT | Auth | Acess Registers  
    

```java  
	@Override    
	public byte[] getAuth0Register() throws IOException, FormatException,CommandNotSupportedException {    
		reader.SectorSelect((byte) 0);    
		return reader.read(Register.AUTH0.getValue());    
	}    
  
	@Override    
	public byte[] getAccessRegister() throws IOException, FormatException,CommandNotSupportedException {    
		reader.SectorSelect((byte) 0);    
		return reader.read(Register.ACCESS.getValue());    
	}    
  
	@Override    
	public byte[] getPTI2CRegister() throws IOException, FormatException,CommandNotSupportedException {    
		reader.SectorSelect((byte) 0);    
		return reader.read(Register.PT_I2C.getValue());    
	}  
```  
  

-------------------------------  

-------------------------------  
  

### in MinimalNtag_I2C_Command  
  

#### Session Register - getSessionRegisters  

- #### describe writing process in brief  
 - params  
 - **SR_Offset** off  
 - offset  of the session registers  
  
 ##### referenced in  
 - Ntag_I2C_Demo.setBoardVersion()   
 - Ntag_I2C_Command.checkPTwritePossible()  

------------  

### in MinimalNtag_I2C_Command  
  

- #### Config Registers  

```java  
	@Override    
	public byte[] getSessionConfigRegisters() throws IOException, FormatException,    
		CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getSessionConfigRegisters is not sSupported for this Phone with NTAG I2C 2k");    
		}    
		answer = mfu.readPages(0xEC8);    
		return answer;    
	}    
  
  
	@Override    
	public byte getSessionConfigRegister(SCR_Offset off) throws IOException,    
		FormatException, CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getSessionConfigRegister is not sSupported for this Phone with NTAG I2C 2k");    
		}    
		return getSessionConfigRegisters()[off.getValue()];    
	}  
```  
  

#### Config Registers - getConfigRegisters  

- #### describe writing process in brief  
 - params (none)  
 ##### referenced in  
 - Ntag_I2C_Demo  
 - readWriteConfig- ### Session Register  
    

```java  
	@Override    
	public byte[] getConfigSessionRegisters() throws IOException, FormatException,    
		CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getConfigSessionRegisters is not Ssupported for this Phone with NTAG I2C 2k");    
		}    
		answer = mfu.readPages(0xE8C);    
		return answer;    
	}    
  
  
	@Override    
	public byte getConfigSessionRegister(CSR_Offset off) throws IOException,    
		FormatException, CommandNotSupportedException {    
		if (tagType == Prod.NTAG_I2C_1k || tagType == Prod.NTAG_I2C_2k) {    
			throw new CommandNotSupportedException("getConfigSessionRegister is not Ssupported for this Phone with NTAG I2C 2k");    
		}    
		return getConfigSessionRegisters()[off.getValue()];    
	}  
```  

#### PT | Auth | Acess Registers  
    

```java  
	@Override    
	public byte[] getAuth0Register() throws IOException, FormatException,CommandNotSupportedException {    
		return mfu.readPages(0xE3);    
	}    
  
	@Override    
	public byte[] getAccessRegister() throws IOException, FormatException,CommandNotSupportedException {    
		return mfu.readPages(0xE4);    
	}    
  
	@Override    
	public byte[] getPTI2CRegister() throws IOException, FormatException,CommandNotSupportedException {    
		return mfu.readPages(0xE7);  
	}  
```  
  
  

---------------------  

----------  
  

### difference bewteen MinimalNtag and Ntag  

- MinimalNtag has **tagType** assertion to block unsupported product  
- MinimalNtag use **readPages(0xE8)** to fetch configuration, and use getPages(0xEC) to fetch session  
- Ntag use **read(Register.Configuration)** to fetch configuration, and use **read(Register.Session/Register.SessionPlus)** to fetch session.  
- Ntag has different sector settings between different product.  
  

--------  
  

### SR_Offset  

```java  
	public enum SR_Offset {    
		NC_REG((byte) 0x00),   
		LAST_NDEF_PAGE((byte) 0x01),  
		SM_REG((byte) 0x02),   
		WDT_LS((byte) 0x03), 		  
		WDT_MS((byte) 0x04),   
		I2C_CLOCK_STR((byte) 0x05),   
		NS_REG((byte) 0x06),   
		FIXED((byte) 0x07);     
	}  
```  

Markdown 6275  bytes 609  words 208  lines Ln 1, Col 0

HTML 4890  characters 542  words 151  paragraphs
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk3NDA5MzM2NF19
-->
