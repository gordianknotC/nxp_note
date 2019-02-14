
# Intent
An intent is an abstract description of an operation to be performed. It can be used with  [startActivity](https://developer.android.com/reference/android/content/Context.html#startActivity(android.content.Intent))`  to launch an  `[Activity](https://developer.android.com/reference/android/app/Activity.html)`,  `[broadcastIntent](https://developer.android.com/reference/android/content/Context.html#sendBroadcast(android.content.Intent))`  to send it to any interested  `[BroadcastReceiver](https://developer.android.com/reference/android/content/BroadcastReceiver.html)`  components, and  `[Context.startService(Intent)](https://developer.android.com/reference/android/content/Context.html#startService(android.content.Intent))`  or`[Context.bindService(Intent, ServiceConnection, int)](https://developer.android.com/reference/android/content/Context.html#bindService(android.content.Intent,%20android.content.ServiceConnection,%20int))`  to communicate with a background  `[Service](https://developer.android.com/reference/android/app/Service.html)`.

An Intent provides a facility for performing late runtime binding between the code in different applications. Its most significant use is in the launching of activities, where it can be thought of as the glue between activities. It is basically a passive data structure holding an abstract description of an action to be performed.

### Intent Structure

The primary pieces of information in an intent are:

-   **action**  -- The general action to be performed, such as  [ACTION_VIEW](https://developer.android.com/reference/android/content/Intent.html#ACTION_VIEW)`,  `[ACTION_EDIT](https://developer.android.com/reference/android/content/Intent.html#ACTION_EDIT)`,  `[ACTION_MAIN](https://developer.android.com/reference/android/content/Intent.html#ACTION_MAIN)`, etc.
    
-   **data**  -- The data to operate on, such as a person record in the contacts database, expressed as a  [Uri](https://developer.android.com/reference/android/net/Uri.html)`.

Some examples of action/data pairs are:

-   **[ACTION_VIEW](https://developer.android.com/reference/android/content/Intent.html#ACTION_VIEW)`  _content://contacts/people/1_**  -- Display information about the person whose identifier is "1".
-  **[ACTION_DIAL](https://developer.android.com/reference/android/content/Intent.html#ACTION_DIAL)`  _content://contacts/people/1_** -- Display the phone dialer with the person filled in.

In addition to these primary attributes, there are a number of secondary attributes that you can also include with an intent:

-   **category**  -- Gives additional information about the action to execute. For example,  [CATEGORY_LAUNCHER](https://developer.android.com/reference/android/content/Intent.html#CATEGORY_LAUNCHER)`  means it should appear in the Launcher as a top-level application, while  `[CATEGORY_ALTERNATIVE](https://developer.android.com/reference/android/content/Intent.html#CATEGORY_ALTERNATIVE)`  means it should be included in a list of alternative actions the user can perform on a piece of data.
    
-   **type**  -- Specifies an explicit type (a **MIME** type) of the intent data. Normally the type is `inferred from the data` itself. By setting this attribute, you disable that evaluation and force an explicit type.
    
-   **component**  -- Specifies an **`explicit name of a component class`** to use for the intent. Normally this is determined by looking at the other information in the intent (the action, data/type, and categories) and matching that with a component that can handle it. `If this attribute is set then none of the evaluation is performed`, and this component is used `exactly as is`. By specifying this attribute, all of the other Intent attributes become optional.
    
-   **extras**  -- This is a  [Bundle](https://developer.android.com/reference/android/os/Bundle.html)  of **any additional** information. This can be used to provide extended information to the component. For example, if we have a action to send an e-mail message, we could also include extra pieces of data here to supply a subject, body, etc.


|| |
|--|--|
| **Intent**() | empty intent |
| **Intent**([String](https://developer.android.com/reference/java/lang/String.html) action, [Uri](https://developer.android.com/reference/android/net/Uri.html) uri) | create intent with given action and uri |
|**Intent**([Context](https://developer.android.com/reference/android/content/Context.html) packageContext, [Class](https://developer.android.com/reference/java/lang/Class.html)<?> cls)|create intent for specific component (explicit using intent)|


> - Intent(**Context** context, **Activity** activity)
> - startActivityForResult(**Intent** intent, **int** requestCode)
> - putExtras(**Intent** src)
> - putExtras(**Bundle** extras)


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
```


> - putExtra(**String name**, **Object** value)
> - getExtra(**String name**, **Object** value)

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

### Intent Resolution

There are two primary forms of intents you will use.

-   **Explicit Intents**  
-   **Implicit Intents**   

When using implicit intents, given such an arbitrary intent we need to know what to do with it. This is handled by the process of _Intent resolution_, which maps an Intent to an [Activity](https://developer.android.com/reference/android/app/Activity.html), [BroadcastReceiver](https://developer.android.com/reference/android/content/BroadcastReceiver.html), or [Service](https://developer.android.com/reference/android/app/Service.html) (or sometimes two or more activities/receivers) that can handle it.

The intent resolution mechanism basically revolves around matching an Intent against all of the **\<intent-filter\>** descriptions in the installed application packages. (**Plus**, in the case of broadcasts, any [BroadcastReceiver](https://developer.android.com/reference/android/content/BroadcastReceiver.html) objects **explicitly** registered with [Context.registerReceiver(BroadcastReceiver, IntentFilter)](https://developer.android.com/reference/android/content/Context.html#registerReceiver(android.content.BroadcastReceiver,%20android.content.IntentFilter)).)

There are three pieces of information in the Intent that are used for resolution: the **action, type**, and **category**.

-   The  **action**, if given, **must be listed** by the component as **one** it handles.
    
-   The  **type**  is retrieved from the Intent's data, if not already supplied in the Intent. Like the action, if a type is included in the intent (either explicitly or implicitly in its data), then this **must be listed** by the component as **one** it handles.
    
-   For **data** that is not a  `content:`  URI and where no explicit type is included in the Intent, instead the  **scheme**  of the intent data (such as  `http:`  or  `mailto:`) is considered. Again like the action, if we are matching a scheme it **must be listed** by the component as **one** it can handle.
-   The  **categories**, if supplied, must  **_all_  be listed** by the activity as categories it handles. That is, if you include the categories  [CATEGORY_LAUNCHER](https://developer.android.com/reference/android/content/Intent.html#CATEGORY_LAUNCHER)  and[CATEGORY_ALTERNATIVE](https://developer.android.com/reference/android/content/Intent.html#CATEGORY_ALTERNATIVE), then you will only resolve to components with an intent that lists  _both_  of those categories. Activities will very often need to support the[CATEGORY_DEFAULT](https://developer.android.com/reference/android/content/Intent.html#CATEGORY_DEFAULT)  so that they can be found by  [Context.startActivity()](https://developer.android.com/reference/android/content/Context.html#startActivity(android.content.Intent))`.

---------
### Standard Activity Actions

| action name | activity action |
|--|--|
| ACTION_MAIN |  Start as a main entry point, does not expect to receive data.|
| ACTION_GET_CONTENT | Activity Action: Allow the user to select a particular kind of data and return it. This is different than [ACTION_PICK](https://developer.android.com/reference/android/content/Intent.html#ACTION_PICK) in that here we just say what kind of data is desired, not a URI of existing data from which the user can pick. An ACTION_GET_CONTENT could allow the user to create the data as it runs (for example taking a picture or recording a sound), let them browse over the web and download the desired data, etc.|
|ACTION_SEARCH_LONG_PRESS|Start action associated with long pressing on the search key.|

------------------------------------------------

### Standard Broadcast Actions
These are the current standard actions that Intent defines for receiving broadcasts (usually through [Context.registerReceiver(BroadcastReceiver, IntentFilter)](https://developer.android.com/reference/android/content/Context.html#registerReceiver(android.content.BroadcastReceiver,%20android.content.IntentFilter)) or a <receiver> tag in a manifest).

------------------------------------------------

### Standard Categories
These are the current standard categories that can be used to further clarify an Intent via [addCategory(String)](https://developer.android.com/reference/android/content/Intent.html#addCategory(java.lang.String)).

|    |    |
| -- | -- |
| CATEGORY_LAUNCHER| Should be displayed in the top-level launcher.|
| CATEGORY_HOME||
| CATEGORY_DEFAULT| This category is mainly used for implicit intents. If your activity wishes to be started by an implicit intent it should include this catetory in its filter.  I think the term "default" should be understood as "default  **_candidate_**".|
| | _Android automatically applies the the CATEGORY_DEFAULT category to all implicit intents passed to startActivity() and startActivityForResult(). So if you want your activity to receive implicit intents, it must include a category for "android.intent.category.DEFAULT" in its intent filters._|

------------------
### setFlags

public [Intent](https://developer.android.com/reference/android/content/Intent.html) setFlags (int flags)

Set special flags controlling how this intent is handled. Most values here depend on the type of component being executed by the Intent, specifically the **_FLAG_ACTIVITY_** flags are all for use with  [Context.startActivity()](https://developer.android.com/reference/android/content/Context.html#startActivity(android.content.Intent))  and the **_FLAG_RECEIVER_** flags are all for use with  [Context.sendBroadcast()](https://developer.android.com/reference/android/content/Context.html#sendBroadcast(android.content.Intent)).




----------------------------------


.
## Intent Sample Code
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

.

 


.

## Parcelable


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
> 實作該二項方法時要 **注意**  寫入及讀取的**順序要一致**
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
eyJoaXN0b3J5IjpbLTI1NDUyNzY0MywtMTUxNDA1NjI3MSwxNz
U2MjU0MTkyLDEzMTYxNjkyODIsLTYyMjQxMzk0LC0xNjYwOTA3
MTc3LDI1Njk4NzU2MCw5NjgyOTc1ODYsMTI2NzM3ODIxOSwxNj
QxNDAxMzE2LDEzMTEwODA2OSw3NjM2NTY2ODJdfQ==
-->