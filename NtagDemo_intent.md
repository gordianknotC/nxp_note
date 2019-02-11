

## How New Intent be Processed
  
- ##### in AuthActivity  
```java
	protected void onNewIntent(Intent nfc_intent) {  
		super.onNewIntent(nfc_intent);  
		// Set the pattern for vibration  
		long pattern[] = { 0, 100 };  

		// Vibrate on new Intent  
		Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE);  
		vibrator.vibrate(pattern, -1);  

		// Get the tag and start the demo  
		Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);  
		demo = new Ntag_I2C_Demo(tag, 
			this, MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		MainActivity.setAuthStatus(demo.ObtainAuthStatus());  

		// This authentication is added in order to avoid authentication problems with old NFC Controllers  
		if(MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
			demo.Auth(MainActivity.getPassword(),
				 AuthStatus.Protected_RW.getValue());  
		}  

		// Set the Auth Status on the screen  
		updateAuthStatus(MainActivity.getAuthStatus());  
	}
```
- ##### MainActivity  
```java
	@Override  
	protected void onNewIntent(Intent nfc_intent) {  
	  super.onNewIntent(nfc_intent);  
	  // Set the pattern for vibration  
	  long pattern[] = { 0, 100 };  
	  
	  // Set the initial auth parameters  
	  mAuthStatus = AuthStatus.Disabled.getValue();  
	  mPassword = null;  
	  
	  // Vibrate on new Intent  
	  Vibrator vibrator = (Vibrator) getSystemService(Context.VIBRATOR_SERVICE);  
	  vibrator.vibrate(pattern, -1);  
	  doProcess(nfc_intent);  
	}

	public void doProcess(Intent nfc_intent) {  
		// m interpreted as mounted
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
	public static void setNfcIntent(Intent intent) {  
	   mIntent = intent;  
	}
```
- ##### RegisterConfigActivity  
```java
	protected void onNewIntent(Intent nfc_intent) {  
		// Set the initial auth parameters  
		MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());  
		MainActivity.setPassword(null);  

		// Set the Write protection check  
		isWriteProtected = false;  

		// Store the intent information  
		MainActivity.setNfcIntent(nfc_intent);  
		final Tag tag = nfc_intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);  
		startDemo(tag, true);  
	}
```
- ##### RegisterSessionActivity  
- ##### in FlashMemoryActivity  
- ##### VersionActivity  
- ##### ReadMemoryActivity  
- ##### ResetMemeoryActivity
```java
	protected void onNewIntent(Intent nfcIntent) {  
		// Set the initial auth parameters  
		MainActivity.setAuthStatus(AuthStatus.Disabled.getValue());  
		MainActivity.setPassword(null);  

		// Store the intent information  
		MainActivity.setNfcIntent(nfcIntent);  

		// Complete the task in a new thread in order to be able to show the dialog  
		Tag tag = nfcIntent.getParcelableExtra(NfcAdapter.EXTRA_TAG);  
		startDemo(tag, true);  
	}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk1ODI1MzQ0N119
-->