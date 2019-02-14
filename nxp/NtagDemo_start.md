

## How Demo be Initialized ( Ntag_I2C )

- ##### in AuthActivity
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
- ##### in FlashMemoryActivity
```java
	private void startDemo(Tag tag, boolean getAuthStatus) {  
		try {  
			if(isAppFW) {  
				bytesToFlash = readFileAssets(indexFW);  
			} else {  
				String path = ((TextView) findViewById(R.id.file_path)).getText().toString();  
				bytesToFlash = readFileMemory(path);  
			}  
		} catch (IOException e) {  
			e.printStackTrace();  

			// Set bytesToFlash to null so that the task is not started  
			bytesToFlash = null;  
		}  
	
		if(bytesToFlash == null || bytesToFlash.length == 0) {  
			Toast.makeText(mContext, "Error could not open File",  
			Toast.LENGTH_SHORT).show();  
			return;  
		}  

		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		if (!demo.isReady())  
			return;  

		// Retrieve the Auth Status  
		if(getAuthStatus)  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  

		// Flashing is only available when the tag is not protected  
		if(MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
			// Launch the thread  
			task = new flashTask();  
			task.execute();  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### MainActivity 
```java

```
- ##### RegisterConfigActivity 
```java
	private void startDemo(final Tag tag, boolean getAuthStatus) {  
	    demo = new Ntag_I2C_Demo(tag, this, 
		    MainActivity.getPassword(), 
		    MainActivity.getAuthStatus());  
		if (!demo.isReady())  
			return;  

		// Calculate the Register Values according to what has been selected by  
		// the user  calcConfiguration();  

		// Retrieve the Auth Status  
		if (getAuthStatus == true) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		if (MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W_SRAM.getValue()) {  
			try {  
				demo.readWriteConfigRegister();  
			} catch (CommandNotSupportedException e) {  
				new AlertDialog.Builder(this)  
					.setMessage("This NFC device does not support the NFC Forum commands needed to access the config register")  
					.setTitle("Command not supported")  
					.setPositiveButton("OK",  
						new DialogInterface.OnClickListener() {  
							@Override  
							public void onClick(DialogInterface dialog, int which) {  

							}  
						}).show();  
				return;  
			}  
			layout_read.setVisibility(View.GONE);  
			layout_buttons.setVisibility(View.VISIBLE);  
			scroll_regs.setVisibility(View.VISIBLE);  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### RegisterSessionActivity 
```java
	private void startDemo(final Tag tag, boolean getAuthStatus) {  
		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		if (!demo.isReady())  
			return;  

		// Calculate the Register Values according to what has been selected by  
		// the user  calcConfiguration();  

		// Retrieve the Auth Status  
		if (getAuthStatus == true) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		if (MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W_SRAM.getValue()) {  
			try {  
				demo.readWriteConfigRegister();  
			} catch (CommandNotSupportedException e) {  
				new AlertDialog.Builder(this)  
					.setMessage(  
					"This NFC device does not support the NFC Forum commands needed to access the config register")  
					.setTitle("Command not supported")  
					.setPositiveButton("OK", new DialogInterface.OnClickListener() {  
						@Override  
						public void onClick(DialogInterface dialog,  
						int which) {  

						}  
					}).show();  
				return;  
			}  
			layout_read.setVisibility(View.GONE);  
			layout_buttons.setVisibility(View.VISIBLE);  
			scroll_regs.setVisibility(View.VISIBLE);  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### ReadMemoryActivity 
```java
	private void startDemo(Tag tag, boolean getAuthStatus) {  
		// Complete the task in a new thread in order to be able to show the dialog  
		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(), 
			MainActivity.getAuthStatus());  
		if(!demo.isReady()) {  
			return;  
		}  

		// Retrieve the Auth Status  
		if(getAuthStatus) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		if(MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Protected_W_SRAM.getValue()) {  
		// Launch the thread  
			new readTask().execute();  
		} else {  
			showAuthDialog();  
		}  
	}
```
- ##### VersionActivity 
```java

```
- ##### ResetMemeoryActivity
```java
	private void startDemo(Tag tag, boolean getAuthStatus) {  
		demo = new Ntag_I2C_Demo(tag, this, 
			MainActivity.getPassword(),  
			MainActivity.getAuthStatus());  
		if(!demo.isReady()) {  
			return;  
		}  

		// Retrieve the Auth Status  
		if(getAuthStatus) {  
			MainActivity.setAuthStatus(demo.ObtainAuthStatus());  
		}  

		// Demo is only available when the tag is not protected  
		if(MainActivity.getAuthStatus() == AuthStatus.Disabled.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Unprotected.getValue()  
		|| MainActivity.getAuthStatus() == AuthStatus.Authenticated.getValue()) {  
		// Launch the thread  
			new resetTask().execute();  
		} else {  
			showAuthDialog();  
		}  
	}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkzMDkxNTkyMV19
-->