
# Ntag_I2C_Demo 
### Properties and Constructor

## Constructor
#### describe process in brief
- params
	* tag **Tag** with which the Demos should be performed  
	* main **MainActivity**
	* passwd **byte[]**
	* authStatus **int**
- return null while tag is null
- initialize reader by **I2C_Enabled_Commands**.get(tag)
- connect reader
- get product info by reader.**getProduct**()

**Main Code**
```java
	public Ntag_I2C_Demo(Tag tag, final Activity main, final byte[] passwd, final int authStatus) {
		try {
			if (tag == null) {
				this.main = null;
				this.tag = null;
				return;
			}
			this.main = main;
			this.tag = tag;

			reader = I2C_Enabled_Commands.get(tag);

			if (reader == null) {
				String message = "The Tag could not be identified or this NFC device does not "
						+ "support the NFC Forum commands needed to access this tag";
				String title = "Communication failed";
				showAlert(message, title);
			} else {
				reader.connect();
			}

			Ntag_Get_Version.Prod prod = reader.getProduct();

			if (!prod.equals(Ntag_Get_Version.Prod.Unknown)) {
				if (prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_1k_Plus)
			     || prod.equals(Ntag_Get_Version.Prod.NTAG_I2C_2k_Plus)) {
					// Auth status gets lost after resetting the demo when we
					// obtain the product we are dealing with
					if(authStatus == AuthStatus.Authenticated.getValue()) {
						reader.authenticatePlus(passwd);
					}
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
```

-----------------------------------------------------------

### isConnected
Checks if the tag is still connected based on the **previously** detected reader. Which means we already know what kind of reader has been detected. 
##### referenced in
- MainActivity
	- onCreate -> setOnTabChangedListener
	- launchNdefDemo
- Ntag | MinimalNtag
	- isConnected
- SpeedTestFragment
	-  StartEEPROMSpeedTest
	- StartSRAMSpeedTest
- UI (RegisterConfigActivity.onClick)
##### example in MainActivity.onCreate
```java
	if (tabId.equalsIgnoreCase("leds") && demo.isConnected()) {  
	  launchDemo(tabId);  
	}
```
##### example in StartXSpeedTest
```java
	if (MainActivity.demo.isReady() && MainActivity.demo.isConnected()) {
		...perform some task
```
**Main Code**
```java
public boolean isConnected() {
	// reader here indicates Ncfa ncfa.isConnected
	return reader.isConnected();
}
```

-----------------------------------------------------------
-----------------------------------------------------------


### isTagPresent
Checks if the tag is still connected based on the tag. Which means we don't know what kind of reader has been resolved. 
#### describe process in brief
- params
	- **Tag** tag 
- get Ndef by Ndef.get(tag) then perform following detection.
- detect if it's connected
	- ndef.isConnected()
	- nfca.isConnected()
	- nfcb.isConnected()
	-  nfcf.isConnected()
	- nfcv.isConnected()
- close ndef and return its result ( isConnected)

	##### referenced in
	- RegisterSessionActivity
	- ResetMemoryActivity
	- RegisterConfigActivity
	- AuthActivity
	##### example:
	```java
	// Capture intent to check whether the operation should be automatically launch or not  
	Tag tag = getIntent().getParcelableExtra(NfcAdapter.EXTRA_TAG);  
	if(tag != null && Ntag_I2C_Demo.isTagPresent(tag)) {  
		startDemo(tag, false);  
	}
	```
	
**Main Code**
```java
public static boolean isTagPresent(Tag tag) {
	final Ndef ndef = Ndef.get(tag);
	if (ndef != null && !ndef.getType().equals("android.ndef.unknown")) {
		try {
			ndef.connect();
			final boolean isConnected = ndef.isConnected();
			ndef.close();
			return isConnected;
		} catch (final IOException e) {
			e.printStackTrace();
			return false;
		}
	} else {
		final NfcA nfca = NfcA.get(tag);
		if (nfca != null) {
			try {
				nfca.connect();
				final boolean isConnected = nfca.isConnected();
				nfca.close();

				return isConnected;
			} catch (final IOException e) {
				e.printStackTrace();
				return false;
			}
		} else {
			final NfcB nfcb = NfcB.get(tag);
			if (nfcb != null) {
				try {
					nfcb.connect();
					final boolean isConnected = nfcb.isConnected();
					nfcb.close();
					return isConnected;
				} catch (final IOException e) {
					e.printStackTrace();
					return false;
				}
			} else {
				final NfcF nfcf = NfcF.get(tag);
				if (nfcf != null) {
					try {
						nfcf.connect();
						final boolean isConnected = nfcf.isConnected();
						nfcf.close();
						return isConnected;
					} catch (final IOException e) {
						e.printStackTrace();
						return false;
					}
				} else {
					final NfcV nfcv = NfcV.get(tag);
					if (nfcv != null) {
						try {
							nfcv.connect();
							final boolean isConnected = nfcv.isConnected();
							nfcv.close();
							return isConnected;
						} catch (final IOException e) {
							e.printStackTrace();
							return false;
						}
					} else {
						return false;
					}
				}
			}
		}
	}
}
```

-----------------------------------------------------------

### isReady
Checks if the demo is ready to be executed.
#### describe process in brief
- tag != null
- reader != null
```java
	public boolean isReady() {
		if (tag != null && reader != null) {
			return true;
		}
		return false;
	}
```
-----------------------------------------------------------
-----------------------------------------------------------


### getProduct
Get the product from detected reader.
```java
public Prod getProduct() throws IOException {
	return reader.getProduct();
}

class MyFlutterActivity : FlutterActivity() {  
  override fun onCreate(savedInstanceState: Bundle?) {  
    super.onCreate(savedInstanceState)  
    GeneratedPluginRegistrant.registerWith(this)  
  }  
}
```

-----------------------------------------------------------

### setCurrentBoardVersion
Set the current board version.
#### describe process in brief
- UI - onBoardVersionSetted
	```java
	 public void onBoardVersionSetted(String ver, String fwver){  
	  //todo: link to ui  
	  VersionInfoActivity.setBoardVersion(ver);  
	  VersionInfoActivity.setBoardFWVersion(fwver);  
	}
	```
##### referenced in
- VersionInfoActivity
	- onActivityResult
	- startDemo

-----------------------------------------------------------


### resetTagMemory
Resets the tag to its delivery values (including config registers).
#### describe process in brief
* params
	* @return **int** indicating number of bytes written
	##### referenced in
	- ResetMemoryActivity
		- doInBackground
	 ##### example - statistic time to reset memory
	```java
	long regTimeOutStart = System.currentTimeMillis();  
	// Reset Tag demo will return the number of bytes written  
	int bytes = demo.resetTagMemory();  
	// Memory erase time statistics  
	timeToResetMemory = System.currentTimeMillis() - regTimeOutStart;  
	```
-----------------------------------------------------------


### getRegister_Settings
Builds a String array for the Registers.
#### describe process in brief
* params
	* register **[]byte** of the Registers 
	* @return **Ntag_I2C_Registers** ( a String Array )

##### referenced in
- Ntag_I2C_Demo
	- readSessionRegisters
	- readWriteConfigRegister
##### example







<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MTgxOTQ0MTJdfQ==
-->