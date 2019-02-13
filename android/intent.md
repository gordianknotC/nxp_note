
putExtras(


```java
intent.putExtras(MainActivity.getmIntent());
```
-------------------------

```java
public void showAuthDialog() {  
	Intent intent = null;  
	intent = new Intent(this, AuthActivity.class);  
	intent.putExtras(MainActivity.getNfcIntent());  
	startActivityForResult(intent, MainActivity.AUTH_REQUEST);  
}
public void showAboutDialog() {  
	Intent intent = null;  
	intent = new Intent(this, VersionInfoActivity.class);  
	if(MainActivity.mIntent != null)  
		intent.putExtras(MainActivity.mIntent);  
	startActivity(intent);  
}
```
-------------------------

putExtra (**String** name, **Any** value)
getExtra(**String** name, **Any** default)
```java
public void sendFeedback() {  
	Intent intent = new Intent(Intent.ACTION_SENDTO);  
	intent.setType("text/plain");  
	intent.putExtra(
		Intent.EXTRA_SUBJECT,  
		this.getString(R.string.email_titel_feedback));  
		
	intent.putExtra(Intent.EXTRA_TEXT, 
		"Android Version: "  
		+ android.os.Build.VERSION.RELEASE + "\nManufacurer: "  
		+ android.os.Build.MANUFACTURER + "\nModel: "  
		+ android.os.Build.MODEL + "\nBrand: " + android.os.Build.BRAND  
		+ "\nDisplay: " + android.os.Build.DISPLAY + "\nProduct: "  
		+ android.os.Build.PRODUCT + "\nIncremental: "  
		+ android.os.Build.VERSION.INCREMENTAL);  
	intent.setData(Uri.parse(this.getString(R.string.support_email)));  
	intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);  
	this.startActivity(intent);  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc5MTQxMzQxNyw3NjM2NTY2ODJdfQ==
-->