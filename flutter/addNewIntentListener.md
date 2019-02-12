

## Sample Code
[fetched from github](https://github.com/flutter/plugins/blob/master/packages/firebase_messaging/android/src/main/java/io/flutter/plugins/firebasemessaging/FirebaseMessagingPlugin.java)
```java
import  android.content.BroadcastReceiver;
import io.flutter.plugin.common.MethodCall;
import io.flutter.plugin.common.MethodChannel;
import io.flutter.plugin.common.MethodChannel.MethodCallHandler;
import io.flutter.plugin.common.MethodChannel.Result;
import io.flutter.plugin.common.PluginRegistry.NewIntentListener;
import io.flutter.plugin.common.PluginRegistry.Registrar;

public class FirebaseMessagingPlugin extends BroadcastReceiver implements MethodCallHandler, NewIntentListener {
	private final Registrar registrar;
	private final MethodChannel channel;

	private static final String CLICK_ACTION_VALUE = "FLUTTER_NOTIFICATION_CLICK";
	private static final String TAG = "FirebaseMessagingPlugin";

	public static void registerWith(Registrar registrar) {
		final MethodChannel channel =
		new MethodChannel(registrar.messenger(), "plugins.flutter.io/firebase_messaging");
		final FirebaseMessagingPlugin plugin = new FirebaseMessagingPlugin(registrar, channel);
		registrar.addNewIntentListener(plugin);
		channel.setMethodCallHandler(plugin);
	}

	private FirebaseMessagingPlugin(Registrar registrar, MethodChannel channel) {
		this.registrar = registrar;
		this.channel = channel;
		FirebaseApp.initializeApp(registrar.context());
		IntentFilter intentFilter = new IntentFilter();
		intentFilter.addAction(FlutterFirebaseInstanceIDService.ACTION_TOKEN);
		intentFilter.addAction(FlutterFirebaseMessagingService.ACTION_REMOTE_MESSAGE);
		LocalBroadcastManager manager = LocalBroadcastManager.getInstance(registrar.context());
		manager.registerReceiver(this, intentFilter);
	}

	// BroadcastReceiver implementation.
	@Override
	public void onReceive(Context context, Intent intent) {
		String action = intent.getAction();
		if (action == null) {
			return;
		}
		String tok_service = FlutterFirebaseInstanceIDService.EXTRA_TOKEN;
		String rem_service = FlutterFirebaseInstanceIDService.ACTION_REMOTE_MESSAGE;
		if (action.equals(tok_service )) {
			String token = intent.getStringExtra(tok_service );
			channel.invokeMethod("onToken", token);
		} else if (action.equals(rem_service )) {
			RemoteMessage message = intent.getParcelableExtra(rem_service );
			Map<String, Object> content = parseRemoteMessage(message);
			channel.invokeMethod("onMessage", content);
		}
	}

	@NonNull
	private Map<String, Object> parseRemoteMessage(RemoteMessage message) {
		...
	}

	@Override
	public void onMethodCall(final MethodCall call, final Result result) {
		.......
	}

	@Override
	public boolean onNewIntent(Intent intent) {
		boolean res = sendMessageFromIntent("onResume", intent);
		if (res && registrar.activity() != null) {
			registrar.activity().setIntent(intent);
		}
		return res;
	}

	/** @return true if intent contained a message to send. */
	private boolean sendMessageFromIntent(String method, Intent intent) {
		if (CLICK_ACTION_VALUE.equals(intent.getAction())
			|| CLICK_ACTION_VALUE.equals(intent.getStringExtra("click_action"))) {
			Map<String, String> message = new HashMap<>();
			Bundle extras = intent.getExtras();

			if (extras == null) {
				return false;
			}

			for (String key : extras.keySet()) {
				Object extra = extras.get(key);
				if (extra != null) {
					message.put(key, extra.toString());
				}
			}

			channel.invokeMethod(method, message);
			return true;
		}
		return false;
	}
}
```

## To Elaborate
### BroadCastReceiver Overview
>[BroadcastReceiver](http://developer.android.com/reference/android/content/BroadcastReceiver.html) 是一個相當常用的類別，它可以用來監聽廣播，並做一些事情。  我們可以先創建一個類別，並且繼承BroadcastReceiver這個類別，實作它要我們做的方法。

```java
	public class MyBroadcastReceiver extends BroadcastReceiver {
		@Override
		public void onReceive(Context context, Intent intent) {
		}
	}
```
>當有人發送廣播給這個BroadcastReceiver時，就會觸發onReceive方法。   **別忘了在AndroidManifest.xml加入這個receiver。**  
```xml
	<receiver android:name=".MyBroadcastReceiver"></receiver>  
```
>下一步，你必須把這個receiver註冊到背景之中。  此時你有兩種選擇。  第一種，在程式片段裡面註冊。  這種的好處是，你可以在特定的頁面裡面在註冊(register)接收器，而且可以註銷(**unregister**)你的接收器。

```java
	//創建一個IntentFilte物件
	IntentFilter intentFilter = new IntentFilter();
	//加入Action的辨識字串
	intentFilter.addAction("Hello");
	//註冊我們創建的BroadcastReceiver
	registerReceiver(new MyBroadcastReceiver() , intentFilter);
	Intent intent = new Intent();
	intent.setAction("Hello");
	sendBroadcast(intent);
```
>以FlutterFireBasePlugin 為例 則使用了 **localBrocastManager**
```java
	IntentFilter intentFilter = new IntentFilter();
	intentFilter.addAction("path.to.action.name");
	LocalBroadcastManager manager = LocalBroadcastManager.getInstance(registrar.context());
	manager.registerReceiver(this, intentFilter);
```


----
### BroadCastReceiver examples

- BroadCastReceiver > register on resum, unregister on pause
  : Example on Network Connection Receiver

```xml
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```
```java
@Override  
public void onResume() { 
	super.onResume(); 
	// 註冊mConnReceiver，並用IntentFilter設置接收的事件類型為網路開關  	
	this.registerReceiver(
		mConnReceiver,
		new IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION)); 
}
@Override  
public void onPause() { 
	super.onPause(); 
	// 解除註冊  
	this.unregisterReceiver(mConnReceiver); 
}
```

- BroadCastReceiver > onReceive
	: Example on Network Connection Receiver
```java
	// BroadcastReceiver implementation.
	@Override
	public void onReceive(Context context, Intent intent) {
		// get action string from intent
		String action = intent.getAction();
		if (action == null) {
			return;
		}
		if (action.equals("action_name") {
			String token = intent.getStringExtra("action_name");
			channel.invokeMethod("onToken", token);
		} else if (action.equals("action_nameB") {
			RemoteMessage message =
			intent.getParcelableExtra("action_nameB");
			Map<String, Object> content = parseRemoteMessage(message);
			channel.invokeMethod("onMessage", content);
		}
	}
```



### LocalBroadCastManager

在寫Android應用時候，有時候或多或少的需要運用廣播來解決某些需求，我們知道廣播有一個特性，就是使用**sendBroadcast**(intent);發送廣播時，手機內所有注冊了**BroadcastReceiver**的應用都可以在接收到這個廣播的，並在**BroadcastReceiver**的**onReceive**()方法進行匹配，而應用是否使用這個廣播則是取決與我們定義的Action與廣播接收者的是否匹配，也就是說平常我們使用的廣播是全局的廣播，誰都有權收到。所以這就有可能產生安全漏洞和隱私數據泄密：

> - 假如別人反編譯你的apk後知道了你的Action，那麼第三方應用就可以發送與該Action匹配的廣播，而你的應用也可以接收到，所以這就有可能被第三方利用這個來搞一些事
> - 同樣假如別人知道了你應用內的Action，當你使用廣播來傳輸一些數據的時候，而其它應用也能接受到這個廣播，通過Action匹配，就有可能獲取到你的私密數據

- **LocalBroadcastManager**.getInstance
	- params 
		- **Context** context 
	- @return **LocalBroadcastManager**

- **LocalBroadcastManager**
	- registerReceiver  | unregisterReceiver
		- params
			- **BroadcastReceiver** receiver
			- **IntentFilter** filter
	- sendBroadCast  
		-  params
			- **Intent** intent

```java
	IntentFilter intentFilter =  new  IntentFilter();
	intentFilter.addAction("path.to.action.name"); 
	LocalBroadcastManager manager = LocalBroadcastManager.getInstance(context); 
	manager.registerReceiver(receiver, intentFilter);
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzM1MTM4MDAsLTcxOTk3NDk3Nyw4NDE4Mz
MzODBdfQ==
-->