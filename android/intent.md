
# Intent
An intent is an abstract description of an operation to be performed. It can be used with  [startActivity](https://developer.android.com/reference/android/content/Context.html#startActivity(android.content.Intent))`  to launch an  `[Activity](https://developer.android.com/reference/android/app/Activity.html)`,  `[broadcastIntent](https://developer.android.com/reference/android/content/Context.html#sendBroadcast(android.content.Intent))`  to send it to any interested  `[BroadcastReceiver](https://developer.android.com/reference/android/content/BroadcastReceiver.html)`  components, and  `[Context.startService(Intent)](https://developer.android.com/reference/android/content/Context.html#startService(android.content.Intent))`  or`[Context.bindService(Intent, ServiceConnection, int)](https://developer.android.com/reference/android/content/Context.html#bindService(android.content.Intent,%20android.content.ServiceConnection,%20int))`  to communicate with a background  `[Service](https://developer.android.com/reference/android/app/Service.html)`.

An Intent provides a facility for performing late runtime binding between the code in different applications. Its most significant use is in the launching of activities, where it can be thought of as the glue between activities. It is basically a passive data structure holding an abstract description of an action to be performed.



Intent(**Context** context, **Activity** activity)
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

原物件
```java
public  class  Albums { 
	private  int userId; 
	private  int id;
	private String title; 
	public Albums(int userId, int id, String title) { 
		this.userId = userId; 
		this.id = id; 
		this.title = title; 
	} 
	public int getUserId() { return userId; } 
	public int getId() { return id; } 
	public String getTitle() { return title; } 
}
```
若要改成Parcelable 主要是實作 Serialize 方法 writeToParcel + readParcel
```java
	writeToParcel(Parcel parcel, Int i){
		parcel.writeInt(userId); parcel.writeInt(id); parcel.writeString(title)
	}
	Albums(Parcel p){
		userId=p.readInt();id=p.readInt();title=p.readString();
	}
```
```kotlin
class Albums : Parcelable {  
	var userId: Int = 0  
		private set  
	var id: Int = 0  
		private set  
	var title: String? = null  
		 private set  
		 
	constructor(userId: Int, id: Int, title: String) {  
		this.userId = userId  
		this.id = id  
		this.title = title  
	}  
	protected constructor(`in`: Parcel) {  
		userId = `in`.readInt()  
		id = `in`.readInt()  
		title = `in`.readString()  
	}
	override fun writeToParcel(parcel: Parcel, i: Int) {  
		//writing order here does matters, must be the same with constructor
		parcel.writeInt(userId)  
		parcel.writeInt(id)  
		parcel.writeString(title)  
	}  
    companion object {  
        @JvmField val CREATOR: Parcelable.Creator<Albums> = object : Parcelable.Creator<Albums> {  
            override fun createFromParcel(`in`: Parcel): Albums {  
                return Albums(`in`)  
            }  
  
            override fun newArray(size: Int): Array<Albums?> {  
                return arrayOfNulls(size)  
            }  
        }  
    }
    override fun describeContents(): Int {   return 0   }    
}
```

send parcelable via intent
```java
// put Parcelable
intent.putExtra("albums", albums);
// put Array<Parcelable>
ArrayList<Albums> list = new ArrayList<>(); 
list.add(albums);
intent.putParcelableArrayListExtra("albums list", list);
```
receive parcelable via intent
```java
Intent  intent = getIntent(); 
Albums albums = intent.getParcelableExtra("albums");
intent.putParcelableArrayListExtra("albums list", list);
```









<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NzYyODk0NzgsOTY4Mjk3NTg2LDEyNj
czNzgyMTksMTY0MTQwMTMxNiwxMzExMDgwNjksNzYzNjU2Njgy
XX0=
-->