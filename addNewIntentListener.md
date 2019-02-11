---


---

<h2 id="sample-code">Sample Code</h2>
<p><a href="https://github.com/flutter/plugins/blob/master/packages/firebase_messaging/android/src/main/java/io/flutter/plugins/firebasemessaging/FirebaseMessagingPlugin.java">fetched from github</a></p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">import</span>  android<span class="token punctuation">.</span>content<span class="token punctuation">.</span>BroadcastReceiver<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodCall<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodChannel<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodChannel<span class="token punctuation">.</span>MethodCallHandler<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodChannel<span class="token punctuation">.</span>Result<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>PluginRegistry<span class="token punctuation">.</span>NewIntentListener<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>PluginRegistry<span class="token punctuation">.</span>Registrar<span class="token punctuation">;</span>

<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">FirebaseMessagingPlugin</span> <span class="token keyword">extends</span> <span class="token class-name">BroadcastReceiver</span> <span class="token keyword">implements</span> <span class="token class-name">MethodCallHandler</span><span class="token punctuation">,</span> NewIntentListener <span class="token punctuation">{</span>
	<span class="token keyword">private</span> <span class="token keyword">final</span> Registrar registrar<span class="token punctuation">;</span>
	<span class="token keyword">private</span> <span class="token keyword">final</span> MethodChannel channel<span class="token punctuation">;</span>

	<span class="token keyword">private</span> <span class="token keyword">static</span> <span class="token keyword">final</span> String CLICK_ACTION_VALUE <span class="token operator">=</span> <span class="token string">"FLUTTER_NOTIFICATION_CLICK"</span><span class="token punctuation">;</span>
	<span class="token keyword">private</span> <span class="token keyword">static</span> <span class="token keyword">final</span> String TAG <span class="token operator">=</span> <span class="token string">"FirebaseMessagingPlugin"</span><span class="token punctuation">;</span>

	<span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">void</span> <span class="token function">registerWith</span><span class="token punctuation">(</span>Registrar registrar<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token keyword">final</span> MethodChannel channel <span class="token operator">=</span>
		<span class="token keyword">new</span> <span class="token class-name">MethodChannel</span><span class="token punctuation">(</span>registrar<span class="token punctuation">.</span><span class="token function">messenger</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token string">"plugins.flutter.io/firebase_messaging"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token keyword">final</span> FirebaseMessagingPlugin plugin <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">FirebaseMessagingPlugin</span><span class="token punctuation">(</span>registrar<span class="token punctuation">,</span> channel<span class="token punctuation">)</span><span class="token punctuation">;</span>
		registrar<span class="token punctuation">.</span><span class="token function">addNewIntentListener</span><span class="token punctuation">(</span>plugin<span class="token punctuation">)</span><span class="token punctuation">;</span>
		channel<span class="token punctuation">.</span><span class="token function">setMethodCallHandler</span><span class="token punctuation">(</span>plugin<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>

	<span class="token keyword">private</span> <span class="token function">FirebaseMessagingPlugin</span><span class="token punctuation">(</span>Registrar registrar<span class="token punctuation">,</span> MethodChannel channel<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token keyword">this</span><span class="token punctuation">.</span>registrar <span class="token operator">=</span> registrar<span class="token punctuation">;</span>
		<span class="token keyword">this</span><span class="token punctuation">.</span>channel <span class="token operator">=</span> channel<span class="token punctuation">;</span>
		FirebaseApp<span class="token punctuation">.</span><span class="token function">initializeApp</span><span class="token punctuation">(</span>registrar<span class="token punctuation">.</span><span class="token function">context</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		IntentFilter intentFilter <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">IntentFilter</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		intentFilter<span class="token punctuation">.</span><span class="token function">addAction</span><span class="token punctuation">(</span>FlutterFirebaseInstanceIDService<span class="token punctuation">.</span>ACTION_TOKEN<span class="token punctuation">)</span><span class="token punctuation">;</span>
		intentFilter<span class="token punctuation">.</span><span class="token function">addAction</span><span class="token punctuation">(</span>FlutterFirebaseMessagingService<span class="token punctuation">.</span>ACTION_REMOTE_MESSAGE<span class="token punctuation">)</span><span class="token punctuation">;</span>
		LocalBroadcastManager manager <span class="token operator">=</span> LocalBroadcastManager<span class="token punctuation">.</span><span class="token function">getInstance</span><span class="token punctuation">(</span>registrar<span class="token punctuation">.</span><span class="token function">context</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		manager<span class="token punctuation">.</span><span class="token function">registerReceiver</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">,</span> intentFilter<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>

	<span class="token comment">// BroadcastReceiver implementation.</span>
	<span class="token annotation punctuation">@Override</span>
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onReceive</span><span class="token punctuation">(</span>Context context<span class="token punctuation">,</span> Intent intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		String action <span class="token operator">=</span> intent<span class="token punctuation">.</span><span class="token function">getAction</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>action <span class="token operator">==</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
			<span class="token keyword">return</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
		String tok_service <span class="token operator">=</span> FlutterFirebaseInstanceIDService<span class="token punctuation">.</span>EXTRA_TOKEN<span class="token punctuation">;</span>
		String rem_service <span class="token operator">=</span> FlutterFirebaseInstanceIDService<span class="token punctuation">.</span>ACTION_REMOTE_MESSAGE<span class="token punctuation">;</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>action<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span>tok_service <span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
			String token <span class="token operator">=</span> intent<span class="token punctuation">.</span><span class="token function">getStringExtra</span><span class="token punctuation">(</span>tok_service <span class="token punctuation">)</span><span class="token punctuation">;</span>
			channel<span class="token punctuation">.</span><span class="token function">invokeMethod</span><span class="token punctuation">(</span><span class="token string">"onToken"</span><span class="token punctuation">,</span> token<span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>action<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span>rem_service <span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
			RemoteMessage message <span class="token operator">=</span> intent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>rem_service <span class="token punctuation">)</span><span class="token punctuation">;</span>
			Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span> Object<span class="token operator">&gt;</span> content <span class="token operator">=</span> <span class="token function">parseRemoteMessage</span><span class="token punctuation">(</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>
			channel<span class="token punctuation">.</span><span class="token function">invokeMethod</span><span class="token punctuation">(</span><span class="token string">"onMessage"</span><span class="token punctuation">,</span> content<span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
	<span class="token punctuation">}</span>

	<span class="token annotation punctuation">@NonNull</span>
	<span class="token keyword">private</span> Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span> Object<span class="token operator">&gt;</span> <span class="token function">parseRemoteMessage</span><span class="token punctuation">(</span>RemoteMessage message<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
	<span class="token punctuation">}</span>

	<span class="token annotation punctuation">@Override</span>
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onMethodCall</span><span class="token punctuation">(</span><span class="token keyword">final</span> MethodCall call<span class="token punctuation">,</span> <span class="token keyword">final</span> Result result<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
	<span class="token punctuation">}</span>

	<span class="token annotation punctuation">@Override</span>
	<span class="token keyword">public</span> <span class="token keyword">boolean</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token keyword">boolean</span> res <span class="token operator">=</span> <span class="token function">sendMessageFromIntent</span><span class="token punctuation">(</span><span class="token string">"onResume"</span><span class="token punctuation">,</span> intent<span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>res <span class="token operator">&amp;&amp;</span> registrar<span class="token punctuation">.</span><span class="token function">activity</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
			registrar<span class="token punctuation">.</span><span class="token function">activity</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">setIntent</span><span class="token punctuation">(</span>intent<span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
		<span class="token keyword">return</span> res<span class="token punctuation">;</span>
	<span class="token punctuation">}</span>

	<span class="token comment">/** @return true if intent contained a message to send. */</span>
	<span class="token keyword">private</span> <span class="token keyword">boolean</span> <span class="token function">sendMessageFromIntent</span><span class="token punctuation">(</span>String method<span class="token punctuation">,</span> Intent intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>CLICK_ACTION_VALUE<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span>intent<span class="token punctuation">.</span><span class="token function">getAction</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span>
			<span class="token operator">||</span> CLICK_ACTION_VALUE<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span>intent<span class="token punctuation">.</span><span class="token function">getStringExtra</span><span class="token punctuation">(</span><span class="token string">"click_action"</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
			Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span> String<span class="token operator">&gt;</span> message <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">HashMap</span><span class="token operator">&lt;</span><span class="token operator">&gt;</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			Bundle extras <span class="token operator">=</span> intent<span class="token punctuation">.</span><span class="token function">getExtras</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

			<span class="token keyword">if</span> <span class="token punctuation">(</span>extras <span class="token operator">==</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
				<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
			<span class="token punctuation">}</span>

			<span class="token keyword">for</span> <span class="token punctuation">(</span>String key <span class="token operator">:</span> extras<span class="token punctuation">.</span><span class="token function">keySet</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
				Object extra <span class="token operator">=</span> extras<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>key<span class="token punctuation">)</span><span class="token punctuation">;</span>
				<span class="token keyword">if</span> <span class="token punctuation">(</span>extra <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
					message<span class="token punctuation">.</span><span class="token function">put</span><span class="token punctuation">(</span>key<span class="token punctuation">,</span> extra<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
				<span class="token punctuation">}</span>
			<span class="token punctuation">}</span>

			channel<span class="token punctuation">.</span><span class="token function">invokeMethod</span><span class="token punctuation">(</span>method<span class="token punctuation">,</span> message<span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token keyword">return</span> <span class="token boolean">true</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
		<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<h2 id="to-elaborate">To Elaborate</h2>
<h3 id="broadcastreceiver-overview">BroadCastReceiver Overview</h3>
<blockquote>
<p><a href="http://developer.android.com/reference/android/content/BroadcastReceiver.html">BroadcastReceiver</a> 是一個相當常用的類別，它可以用來監聽廣播，並做一些事情。  我們可以先創建一個類別，並且繼承BroadcastReceiver這個類別，實作它要我們做的方法。</p>
</blockquote>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">MyBroadcastReceiver</span> <span class="token keyword">extends</span> <span class="token class-name">BroadcastReceiver</span> <span class="token punctuation">{</span>
		<span class="token annotation punctuation">@Override</span>
		<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onReceive</span><span class="token punctuation">(</span>Context context<span class="token punctuation">,</span> Intent intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token punctuation">}</span>
	<span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>當有人發送廣播給這個BroadcastReceiver時，就會觸發onReceive方法。   <strong>別忘了在AndroidManifest.xml加入這個receiver。</strong></p>
</blockquote>
<pre class=" language-xml"><code class="prism  language-xml">	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>receiver</span> <span class="token attr-name"><span class="token namespace">android:</span>name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>.MyBroadcastReceiver<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>receiver</span><span class="token punctuation">&gt;</span></span>  
</code></pre>
<blockquote>
<p>下一步，你必須把這個receiver註冊到背景之中。  此時你有兩種選擇。  第一種，在程式片段裡面註冊。  這種的好處是，你可以在特定的頁面裡面在註冊(register)接收器，而且可以註銷(<strong>unregister</strong>)你的接收器。</p>
</blockquote>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">//創建一個IntentFilte物件</span>
	IntentFilter intentFilter <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">IntentFilter</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token comment">//加入Action的辨識字串</span>
	intentFilter<span class="token punctuation">.</span><span class="token function">addAction</span><span class="token punctuation">(</span><span class="token string">"Hello"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token comment">//註冊我們創建的BroadcastReceiver</span>
	<span class="token function">registerReceiver</span><span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">MyBroadcastReceiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">,</span> intentFilter<span class="token punctuation">)</span><span class="token punctuation">;</span>
	Intent intent <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Intent</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	intent<span class="token punctuation">.</span><span class="token function">setAction</span><span class="token punctuation">(</span><span class="token string">"Hello"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token function">sendBroadcast</span><span class="token punctuation">(</span>intent<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<blockquote>
<p>以FlutterFireBasePlugin 為例 則使用了 <strong>localBrocastManager</strong></p>
</blockquote>
<pre class=" language-java"><code class="prism  language-java">	IntentFilter intentFilter <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">IntentFilter</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	intentFilter<span class="token punctuation">.</span><span class="token function">addAction</span><span class="token punctuation">(</span><span class="token string">"path.to.action.name"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	LocalBroadcastManager manager <span class="token operator">=</span> LocalBroadcastManager<span class="token punctuation">.</span><span class="token function">getInstance</span><span class="token punctuation">(</span>registrar<span class="token punctuation">.</span><span class="token function">context</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	manager<span class="token punctuation">.</span><span class="token function">registerReceiver</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">,</span> intentFilter<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h3 id="broadcastreceiver-examples">BroadCastReceiver examples</h3>
<ul>
<li>
<dl>
<dt>BroadCastReceiver &gt; register on resum, unregister on pause</dt>
<dd>Example on Network Connection Receiver</dd>
</dl>
</li>
</ul>
<pre class=" language-xml"><code class="prism  language-xml">	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>uses-permission</span> <span class="token attr-name"><span class="token namespace">android:</span>name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>android.permission.ACCESS_NETWORK_STATE<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
</code></pre>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Override</span>  
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onResume</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> 
	<span class="token keyword">super</span><span class="token punctuation">.</span><span class="token function">onResume</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
	<span class="token comment">// 註冊mConnReceiver，並用IntentFilter設置接收的事件類型為網路開關  	</span>
	<span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">registerReceiver</span><span class="token punctuation">(</span>
		mConnReceiver<span class="token punctuation">,</span>
		<span class="token keyword">new</span> <span class="token class-name">IntentFilter</span><span class="token punctuation">(</span>ConnectivityManager<span class="token punctuation">.</span>CONNECTIVITY_ACTION<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
<span class="token punctuation">}</span>
<span class="token annotation punctuation">@Override</span>  
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onPause</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> 
	<span class="token keyword">super</span><span class="token punctuation">.</span><span class="token function">onPause</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
	<span class="token comment">// 解除註冊  </span>
	<span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">unregisterReceiver</span><span class="token punctuation">(</span>mConnReceiver<span class="token punctuation">)</span><span class="token punctuation">;</span> 
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<dl>
<dt>BroadCastReceiver &gt; onReceive</dt>
<dd>Example on Network Connection Receiver</dd>
</dl>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">// BroadcastReceiver implementation.</span>
	<span class="token annotation punctuation">@Override</span>
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onReceive</span><span class="token punctuation">(</span>Context context<span class="token punctuation">,</span> Intent intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token comment">// get action string from intent</span>
		String action <span class="token operator">=</span> intent<span class="token punctuation">.</span><span class="token function">getAction</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>action <span class="token operator">==</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
			<span class="token keyword">return</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>action<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span><span class="token string">"action_name"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
			String token <span class="token operator">=</span> intent<span class="token punctuation">.</span><span class="token function">getStringExtra</span><span class="token punctuation">(</span><span class="token string">"action_name"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			channel<span class="token punctuation">.</span><span class="token function">invokeMethod</span><span class="token punctuation">(</span><span class="token string">"onToken"</span><span class="token punctuation">,</span> token<span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>action<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span><span class="token string">"action_nameB"</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
			RemoteMessage message <span class="token operator">=</span>
			intent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span><span class="token string">"action_nameB"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span> Object<span class="token operator">&gt;</span> content <span class="token operator">=</span> <span class="token function">parseRemoteMessage</span><span class="token punctuation">(</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>
			channel<span class="token punctuation">.</span><span class="token function">invokeMethod</span><span class="token punctuation">(</span><span class="token string">"onMessage"</span><span class="token punctuation">,</span> content<span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
	<span class="token punctuation">}</span>
</code></pre>
<h3 id="localbroadcastmanager">LocalBroadCastManager</h3>
<p>在寫Android應用時候，有時候或多或少的需要運用廣播來解決某些需求，我們知道廣播有一個特性，就是使用<strong>sendBroadcast</strong>(intent);發送廣播時，手機內所有注冊了<strong>BroadcastReceiver</strong>的應用都可以在接收到這個廣播的，並在<strong>BroadcastReceiver</strong>的<strong>onReceive</strong>()方法進行匹配，而應用是否使用這個廣播則是取決與我們定義的Action與廣播接收者的是否匹配，也就是說平常我們使用的廣播是全局的廣播，誰都有權收到。所以這就有可能產生安全漏洞和隱私數據泄密：</p>
<blockquote>
<ul>
<li>假如別人反編譯你的apk後知道了你的Action，那麼第三方應用就可以發送與該Action匹配的廣播，而你的應用也可以接收到，所以這就有可能被第三方利用這個來搞一些事</li>
<li>同樣假如別人知道了你應用內的Action，當你使用廣播來傳輸一些數據的時候，而其它應用也能接受到這個廣播，通過Action匹配，就有可能獲取到你的私密數據</li>
</ul>
</blockquote>
<ul>
<li>
<p><strong>LocalBroadcastManager</strong>.getInstance</p>
<ul>
<li>params
<ul>
<li><strong>Context</strong> context</li>
</ul>
</li>
<li>@return <strong>LocalBroadcastManager</strong></li>
</ul>
</li>
<li>
<p><strong>LocalBroadcastManager</strong></p>
<ul>
<li>methods
<ul>
<li>registerReceiver  | unregisterReceiver
<ul>
<li>params
<ul>
<li><strong>BroadcastReceiver</strong> receiver</li>
<li><strong>IntentFilter</strong> filter</li>
</ul>
</li>
</ul>
</li>
<li>sendBroadCast
<ul>
<li>params
<ul>
<li><strong>Intent</strong> intent</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>

