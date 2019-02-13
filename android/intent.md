
putExtras(**Intent** src)

```java
intent.putExtras(MainActivity.getmIntent());
```
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
----------------------------------

putExtra (**String** name, **Any** value)
getExtra(**String** name, **Any** default)

```java
Intent i = new Intent(FirstScreen.this, SecondScreen.class);   
String value = null;
i.putExtra("STRING_I_NEED", value);
String newString;
if (savedInstanceState == null) {
	Bundle extras = getIntent().getExtras();
	if(extras == null) {
		newString= null;
	} else {
		newString= extras.getString("STRING_I_NEED");
	}
} else {
	newString= (String) savedInstanceState.getSerializable("STRING_I_NEED");
}
``` 

----------------------------------

### Pass Items from One Activity to Another
**Item class :**
```java
public class Item implements Serializable
```
**In first Activity :**
```java
Intent intent = new Intent(this, Activity2.class);
intent.putExtra("items", items);
startActivity(intent);
```
**In Second Activity (Activity2):**
```java
ArrayList<Item> items = 
	(ArrayList<Item>) getIntent().getExtras()
    .getSerializable("items");
```

----------------------------------

### Parcelable

將物件序列化的方式有兩種，一是Java原生的Serializable，二是Android特有的Parcelable，而Parcelable的處理速度較快，且處理時不會產生大量暫時變數讓系統一直GC，在傳遞大量資料時會有較好的效能表現。

```java
public  class  Albums { 
	private  int userId; 
	private  int id;
	private String title; 
	public Albums(int userId, int id, String title) { 
	this.userId = userId; 
	this.id = id; 
	this.title = title; } 
	public int getUserId() { return userId; } 
	public int getId() { return id; } 
	public String getTitle() { return title; } 
}
```








<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYxNzQ1NTEwMywxNjQxNDAxMzE2LDEzMT
EwODA2OSw3NjM2NTY2ODJdfQ==
-->