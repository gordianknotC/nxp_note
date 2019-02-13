
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
		parcel.writeInt(userId); parcel.writeInt(
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
 
    override fun describeContents(): Int {   return 0   }  
  
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
}
```









<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NDA4MjQ4NTksMTY0MTQwMTMxNiwxMz
ExMDgwNjksNzYzNjU2NjgyXX0=
-->