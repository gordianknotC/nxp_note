

# How Ntag I2C Demo / Tag be Initialized  
  
#### Ntag_I2C_demo  
Ntag_I2C_demo constructor  
```java  
public Ntag_I2C_Demo(Tag tag, final Activity main, final byte[] passwd, final int authStatus) {
}
```  
  
##### in AuthActivity  
- onCreate  
	```java  
	mTag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG); 
	if(mTag != null && Ntag_I2C_Demo.isTagPresent(mTag)) { 
		demo = new Ntag_I2C_Demo( 
			mTag, this,             
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());  
		} 
	 ``` 

- onNewIntent  
	```java  
		Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);
		demo = new Ntag_I2C_Demo( tag, this,             
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());  
	```  

- startDemo  
	```java  
		private void startDemo(Tag tag) {      
			// This authentication is added in order to avoid authentication problems with old NFC Controllers    
			if(MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {    
				demo.Auth(MainActivity.getPassword(), AuthStatus.Protected_RW.getValue());    
			}    
			if(demo != null && demo.isReady()) {    
				// Launch the thread    
				task = new authTask();    
				task.execute();    
			}    
		}  
	```  


##### in FlashMemoryActivity  
 - onNewIntent  
	```java  
		Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG); startDemo(tag, true); 
	``` 

- startDemo  
	```java  
		demo = new Ntag_I2C_Demo( tag, this,             
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());  
		if (!demo.isReady()) return; 
	``` 

##### MainActivity  
 - onCreate  
	```java  
		// Initialize the demo in order to handle tab change events 
		demo = new Ntag_I2C_Demo(null, this, null, 0); 
		mAdapter = NfcAdapter.getDefaultAdapter(this); 
		setNfcForeground(); 
		checkNFC(); 
	``` 

	- onNewIntent  
	```java  
		protected void onNewIntent(Intent nfc_intent) { 
			super.onNewIntent(nfc_intent); 
			// Set the pattern for vibration long pattern[] = { 0, 100 };            
			// Set the initial auth parameters  
			mAuthStatus = AuthStatus.Disabled.getValue(); 
			mPassword = null;            
			// Vibrate on new Intent  
			Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE); 		
			vibrator.vibrate(pattern, -1); 
			doProcess(nfc_intent); 
		} 
	``` 

	- doProcess  
	```java  
		public void doProcess(Intent nfc_intent) { 
			mIntent = nfc_intent; 
			Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG); 
			demo = new Ntag_I2C_Demo(tag, this, mPassword, mAuthStatus); 
			if (demo.isReady()) { 
				// Retrieve Auth Status before doing any operation 
				mAuthStatus = obtainAuthStatus(); 
				String currTab = mTabHost.getCurrentTabTag(); 
				launchDemo(currTab); 
			} 
		} 
	``` 


##### RegisterConfigActivity  
  - onCreate  
	```java  
		// Capture intent to check whether the operation should be automatically launch or not    
		Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);    
		if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {    
			startDemo(tag, false);    
		}    

		// Add Foreground dispatcher    
		mAdapter = NfcAdapter.getDefaultAdapter(this);    
		pendingIntent = PendingIntent.getActivity(  
			this, 0,      
			new Intent(this, getClass()
		).addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP), 0);    
		return; // end onCreate  
	```  

- startDemo  
	```java  
		private void startDemo(final Tag tag, boolean getAuthStatus) {      
			demo = new Ntag_I2C_Demo(tag, this,   
				MainActivity.getPassword(), 
				MainActivity.getAuthStatus());    
			if (!demo.isReady())    
				return;  
	```  

##### RegisterSessionActivity  
- onCreate  
	```java  
		// Capture intent to check whether the operation should be automatically launch or not    
		Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);    
		if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {    
			startDemo(tag, false);    
		}  
	```  

- startDemo  
	```java  
		private void startDemo(final Tag tag, boolean getAuthStatus) {      
			demo = new Ntag_I2C_Demo(tag, this,   
			MainActivity.getPassword(),   
			MainActivity.getAuthStatus());    
			if(!demo.isReady()) {    
				return;    
			}  
	```  

- onNewIntent  
	```java  
		protected void onNewIntent(Intent nfcIntent) {      
			// Set the initial auth parameters    
			MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());    
			MainActivity.setPassword(null);    

			// Store the intent information    
			MainActivity.setNfcIntent(nfcIntent);    
			Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG);    
			startDemo(tag, true);    
		}  
	```


##### ReadMemoryActivity  
- onNewIntent  
	```java  
		protected void onNewIntent(Intent nfcIntent) {      
			// Set the initial auth parameters    
			MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());    
			MainActivity.setPassword(null);    

			// Store the intent information    
			MainActivity.setNfcIntent(nfcIntent);    

			Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG);    
			startDemo(tag, true);    
		}  
	```  

	- startDemo  
	```java  
		private void startDemo(Tag tag, boolean getAuthStatus) {       
			// Complete the task in a new thread in order to be able to show the dialog    
			demo = new Ntag_I2C_Demo(  
				tag, this,        
				MainActivity.getPassword(),   
				MainActivity.getAuthStatus());    
			if(!demo.isReady()) {    
				return;    
			}    
		...  
	} 
	```


- ##### VersionActivity  
  the same as aboves



-----------


## Handling null Tag

```java
	// within onCreate...
	Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);    
	if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {    
		startDemo(tag, false);    
	}  
```

-----------------


## Initialize Ntag_IC2_Command
**Ntag_IC2_Command** is initialized on only one parameter. Unlike **MinimalNtag_I2C_Command** required two parameters, which is **Tag** tag and **Prod** tagType, since tagType in **Ntag_I2C_Command** can be fetched by **Ntag_Get_Version**.
1) **Tag** tag
```java
public static I2C_Enabled_Commands get(Tag tag) throws IOException, InterruptedException {  
	byte[] answer;  
	byte[] command = new byte[2];  
	NfcA nfca = NfcA.get(tag);  
	Prod prod;  

	// Check for support of setTimout to be able to send an efficient  
	// sector_select - select minimal implementation if not supported  nfca.setTimeout(20);  
	// check for timeout  
	if (nfca.getTimeout() < 50) {  
		// check if GetVersion is supported  
		try {  
			nfca.connect();  
			command = new byte[1];  
			command[0] = (byte) 0x60; // GET_VERSION  
			answer = nfca.transceive(command);  
			prod = (new Ntag_Get_Version(answer)).Get_Product();  
			nfca.close();  
			if (prod == Prod.NTAG_I2C_1k || prod == Prod.NTAG_I2C_2k  
			|| prod == Prod.NTAG_I2C_1k_T || prod == Prod.NTAG_I2C_2k_T  
			|| prod == Prod.NTAG_I2C_1k_V || prod == Prod.NTAG_I2C_2k_V  
			|| prod == Prod.NTAG_I2C_1k_Plus || prod == Prod.NTAG_I2C_2k_Plus) {  
				return new Ntag_I2C_Commands(tag);  
			}  
		} catch (Exception e) {  
			e.printStackTrace();  
			nfca.close();  

			// check if sector select is supported  
			try {  
				nfca.connect();  
				command = new byte[2];  
				command[0] = (byte) 0xC2; //SECTOR_SELECT  
				command[1] = (byte) 0xFF;  
				answer = nfca.transceive(command);  
				nfca.close();  
				return new Ntag_I2C_Commands(tag);  

			} catch (Exception e2) {  
				e.printStackTrace();  
				nfca.close();  
			}  
		}  
	}

```


-----------

## Initialize MinimalNtag_IC2_Command
**MinimalNtag_IC2_Command** is initialized by two parameters
1) **Tag** tag
2) **Prod** tagType (product)
	- generally speaking, tagType was fetched by Ntag_Get_Version(answer)
	- but there are some exceptions occurs in MininalNtag
  **todo: rewrite a new Ntag_Get_Version to consider those exceptions**
 
 
```java
	//check if we can use the minimal Version  
	MifareUltralight mfu = MifareUltralight.get(tag);  
	try {  
		mfu.connect();  
		command = new byte[1];  
		command[0] = (byte) 0x60; // GET_VERSION  
		answer = mfu.transceive(command);  
		prod = (new Ntag_Get_Version(answer)).Get_Product();  
		mfu.close();  
		if (prod == Prod.NTAG_I2C_1k || prod == Prod.NTAG_I2C_2k  
		|| prod == Prod.NTAG_I2C_1k_T || prod == Prod.NTAG_I2C_2k_T  
		|| prod == Prod.NTAG_I2C_1k_V || prod == Prod.NTAG_I2C_2k_V  
		|| prod == Prod.NTAG_I2C_1k_Plus || prod == Prod.NTAG_I2C_2k_Plus) {  
			return new MinimalNtag_I2C_Commands(tag, prod);  
		}  
	} catch (Exception e) {  
		e.printStackTrace();  
		mfu.close();  
		try {  
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

					for (int i = 0; i < 4; i++) {  
						if (answer[i] != 0x00) {  
							prod = Prod.NTAG_I2C_1k_Plus;  
							return new MinimalNtag_I2C_Commands(tag, prod);  
						}  
					}  
					prod = Prod.NTAG_I2C_1k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				} catch (Exception e2) {  
					e2.printStackTrace();  
					mfu.close();  
					prod = Prod.NTAG_I2C_1k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				}  
			} else if (answer[0] == (byte) 0x04 && answer[12] == (byte) 0xE1  
				&& answer[13] == (byte) 0x10 && answer[14] == (byte) 0xEA  
				&& answer[15] == (byte) 0x00) {  
				// Try to read session registers to differentiate between standard and PLUS products  
				try {  
					answer = mfu.readPages(0xEC);  
					mfu.close();  
					for (int i = 0; i < 4; i++) {  
						if (answer[i] != 0x00) {  
							prod = Prod.NTAG_I2C_2k_Plus;  
							return new MinimalNtag_I2C_Commands(tag, prod);  
						}  
					}  
					prod = Prod.NTAG_I2C_2k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				} catch (Exception e2) {  
					e2.printStackTrace();  
					mfu.close();  
					prod = Prod.NTAG_I2C_2k;  
					return new MinimalNtag_I2C_Commands(tag, prod);  
				}  
			} else {  
				mfu.close();  
				return new MinimalNtag_I2C_Commands(tag, Prod.NTAG_I2C_1k_Plus);  
			}  
		} catch (Exception e1) {  
			e1.printStackTrace();  
			mfu.close();  
		}  
	}  
	return new MinimalNtag_I2C_Commands(tag, Prod.NTAG_I2C_1k_Plus);

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMzE5OTc1MzhdfQ==
-->