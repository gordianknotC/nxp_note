<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>addActivityResultListener</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
  <style type="text/css">
	blockquote {
	    color: #02151d8a;
	    padding-left: 1.5em;
	    border-left: 5px solid #0000001a;
	}
  </style>
</head>

<body class="stackedit">
  <div class="stackedit__html"><h2 id="sample-code">Sample Code</h2>
<p><a href="https://github.com/marekchen/flutter_qq/blob/master/android/src/main/java/com/github/marekchen/flutterqq/FlutterQqPlugin.java">fetched from github</a></p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">package</span> com<span class="token punctuation">.</span>github<span class="token punctuation">.</span>marekchen<span class="token punctuation">.</span>flutterqq<span class="token punctuation">;</span>

<span class="token keyword">import</span> android<span class="token punctuation">.</span>content<span class="token punctuation">.</span>Intent<span class="token punctuation">;</span>
<span class="token keyword">import</span> android<span class="token punctuation">.</span>os<span class="token punctuation">.</span>Bundle<span class="token punctuation">;</span>
<span class="token keyword">import</span> android<span class="token punctuation">.</span>os<span class="token punctuation">.</span>Handler<span class="token punctuation">;</span>
<span class="token keyword">import</span> android<span class="token punctuation">.</span>os<span class="token punctuation">.</span>Looper<span class="token punctuation">;</span>
<span class="token keyword">import</span> android<span class="token punctuation">.</span>util<span class="token punctuation">.</span>Log<span class="token punctuation">;</span>

<span class="token keyword">import</span> org<span class="token punctuation">.</span>json<span class="token punctuation">.</span>JSONObject<span class="token punctuation">;</span>
<span class="token keyword">import</span> java<span class="token punctuation">.</span>util<span class="token punctuation">.</span>ArrayList<span class="token punctuation">;</span>
<span class="token keyword">import</span> java<span class="token punctuation">.</span>util<span class="token punctuation">.</span>HashMap<span class="token punctuation">;</span>
<span class="token keyword">import</span> java<span class="token punctuation">.</span>util<span class="token punctuation">.</span>Map<span class="token punctuation">;</span>

<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodChannel<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodChannel<span class="token punctuation">.</span>MethodCallHandler<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodChannel<span class="token punctuation">.</span>Result<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>MethodCall<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>PluginRegistry<span class="token punctuation">;</span>
<span class="token keyword">import</span> io<span class="token punctuation">.</span>flutter<span class="token punctuation">.</span>plugin<span class="token punctuation">.</span>common<span class="token punctuation">.</span>PluginRegistry<span class="token punctuation">.</span>Registrar<span class="token punctuation">;</span>

<span class="token comment">/**
 * FlutterQqPlugin
 */</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">FlutterQqPlugin</span> <span class="token keyword">implements</span> <span class="token class-name">MethodCallHandler</span> <span class="token punctuation">{</span>
    <span class="token keyword">private</span> Registrar registrar<span class="token punctuation">;</span>
    <span class="token keyword">private</span> <span class="token keyword">boolean</span> isLogin<span class="token punctuation">;</span>

    <span class="token keyword">private</span> <span class="token function">FlutterQqPlugin</span><span class="token punctuation">(</span>Registrar registrar<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">this</span><span class="token punctuation">.</span>registrar <span class="token operator">=</span> registrar<span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token comment">/**
     * Plugin registration.
     */</span>
    <span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">void</span> <span class="token function">registerWith</span><span class="token punctuation">(</span>Registrar registrar<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token keyword">final</span> MethodChannel channel <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">MethodChannel</span><span class="token punctuation">(</span>registrar<span class="token punctuation">.</span><span class="token function">messenger</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token string">"flutter_qq"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">final</span> FlutterQqPlugin instance <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">FlutterQqPlugin</span><span class="token punctuation">(</span>registrar<span class="token punctuation">)</span><span class="token punctuation">;</span>
        channel<span class="token punctuation">.</span><span class="token function">setMethodCallHandler</span><span class="token punctuation">(</span>instance<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token annotation punctuation">@Override</span>
    <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onMethodCall</span><span class="token punctuation">(</span>MethodCall call<span class="token punctuation">,</span> Result result<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        OneListener listener <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">OneListener</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        registrar<span class="token punctuation">.</span><span class="token function">addActivityResultListener</span><span class="token punctuation">(</span>listener<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token keyword">switch</span> <span class="token punctuation">(</span>call<span class="token punctuation">.</span>method<span class="token punctuation">)</span> <span class="token punctuation">{</span>
           <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">registerQQ</span><span class="token punctuation">(</span>MethodCall call<span class="token punctuation">,</span> Result result<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        String mAppid <span class="token operator">=</span> call<span class="token punctuation">.</span><span class="token function">argument</span><span class="token punctuation">(</span><span class="token string">"appId"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        mTencent <span class="token operator">=</span> Tencent<span class="token punctuation">.</span><span class="token function">createInstance</span><span class="token punctuation">(</span>mAppid<span class="token punctuation">,</span> registrar<span class="token punctuation">.</span><span class="token function">context</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        result<span class="token punctuation">.</span><span class="token function">success</span><span class="token punctuation">(</span><span class="token boolean">true</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">isQQInstalled</span><span class="token punctuation">(</span>MethodCall call<span class="token punctuation">,</span> Result result<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        result<span class="token punctuation">.</span><span class="token function">success</span><span class="token punctuation">(</span>mTencent<span class="token punctuation">.</span><span class="token function">isQQInstalled</span><span class="token punctuation">(</span>registrar<span class="token punctuation">.</span><span class="token function">activeContext</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">login</span><span class="token punctuation">(</span>MethodCall call<span class="token punctuation">,</span> <span class="token keyword">final</span> OneListener listener<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        String scopes <span class="token operator">=</span> <span class="token punctuation">(</span>String<span class="token punctuation">)</span> call<span class="token punctuation">.</span><span class="token function">argument</span><span class="token punctuation">(</span><span class="token string">"scopes"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
        mTencent<span class="token punctuation">.</span><span class="token function">login</span><span class="token punctuation">(</span>
			registrar<span class="token punctuation">.</span><span class="token function">activity</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
			scopes <span class="token operator">==</span> null <span class="token operator">?</span> <span class="token string">"get_simple_userinfo"</span> <span class="token operator">:</span> scopes<span class="token punctuation">,</span> 
			listener<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">doShareToQQ</span><span class="token punctuation">(</span>MethodCall call<span class="token punctuation">,</span> <span class="token keyword">final</span> OneListener listener<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">doShareToQzone</span><span class="token punctuation">(</span>MethodCall call<span class="token punctuation">,</span> <span class="token keyword">final</span> OneListener listener<span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">private</span> <span class="token keyword">class</span> <span class="token class-name">OneListener</span> <span class="token keyword">implements</span> <span class="token class-name">IUiListener</span><span class="token punctuation">,</span> PluginRegistry<span class="token punctuation">.</span>ActivityResultListener <span class="token punctuation">{</span>
        <span class="token keyword">private</span> Result result<span class="token punctuation">;</span>
        <span class="token keyword">void</span> <span class="token function">setResult</span><span class="token punctuation">(</span>Result result<span class="token punctuation">)</span> <span class="token punctuation">{</span>
            <span class="token keyword">this</span><span class="token punctuation">.</span>result <span class="token operator">=</span> result<span class="token punctuation">;</span>
        <span class="token punctuation">}</span>

        <span class="token annotation punctuation">@Override</span>
        <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onComplete</span><span class="token punctuation">(</span>Object response<span class="token punctuation">)</span> <span class="token punctuation">{</span>
            Log<span class="token punctuation">.</span><span class="token function">i</span><span class="token punctuation">(</span><span class="token string">"FlutterQqPlugin"</span><span class="token punctuation">,</span> response<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span> Object<span class="token operator">&gt;</span> re <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">HashMap</span><span class="token operator">&lt;</span><span class="token operator">&gt;</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
            re<span class="token punctuation">.</span><span class="token function">put</span><span class="token punctuation">(</span><span class="token string">"Code"</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            re<span class="token punctuation">.</span><span class="token function">put</span><span class="token punctuation">(</span><span class="token string">"Message"</span><span class="token punctuation">,</span> response<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            result<span class="token punctuation">.</span><span class="token function">success</span><span class="token punctuation">(</span>re<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>

        <span class="token annotation punctuation">@Override</span>
        <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onError</span><span class="token punctuation">(</span>UiError uiError<span class="token punctuation">)</span> <span class="token punctuation">{</span>
            Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span> Object<span class="token operator">&gt;</span> re <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">HashMap</span><span class="token operator">&lt;</span><span class="token operator">&gt;</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            re<span class="token punctuation">.</span><span class="token function">put</span><span class="token punctuation">(</span><span class="token string">"Code"</span><span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            re<span class="token punctuation">.</span><span class="token function">put</span><span class="token punctuation">(</span><span class="token string">"Message"</span><span class="token punctuation">,</span> <span class="token string">"errorCode:"</span>  <span class="token punctuation">)</span><span class="token punctuation">;</span>
            result<span class="token punctuation">.</span><span class="token function">success</span><span class="token punctuation">(</span>re<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>

        <span class="token annotation punctuation">@Override</span>
        <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onCancel</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
            Log<span class="token punctuation">.</span><span class="token function">w</span><span class="token punctuation">(</span><span class="token string">"FlutterQqPlugin"</span><span class="token punctuation">,</span> <span class="token string">"error:cancel"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            Map<span class="token operator">&lt;</span>String<span class="token punctuation">,</span> Object<span class="token operator">&gt;</span> re <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">HashMap</span><span class="token operator">&lt;</span><span class="token operator">&gt;</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            re<span class="token punctuation">.</span><span class="token function">put</span><span class="token punctuation">(</span><span class="token string">"Code"</span><span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            re<span class="token punctuation">.</span><span class="token function">put</span><span class="token punctuation">(</span><span class="token string">"Message"</span><span class="token punctuation">,</span> <span class="token string">"cancel"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            result<span class="token punctuation">.</span><span class="token function">success</span><span class="token punctuation">(</span>re<span class="token punctuation">)</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>

        <span class="token annotation punctuation">@Override</span>
        <span class="token keyword">public</span> <span class="token keyword">boolean</span> <span class="token function">onActivityResult</span><span class="token punctuation">(</span><span class="token keyword">int</span> requestCode<span class="token punctuation">,</span> <span class="token keyword">int</span> resultCode<span class="token punctuation">,</span> Intent data<span class="token punctuation">)</span> <span class="token punctuation">{</span>
            <span class="token keyword">if</span> <span class="token punctuation">(</span>requestCode <span class="token operator">==</span> Constants<span class="token punctuation">.</span>REQUEST_LOGIN <span class="token operator">||</span>
                    requestCode <span class="token operator">==</span> Constants<span class="token punctuation">.</span>REQUEST_QQ_SHARE <span class="token operator">||</span>
                    requestCode <span class="token operator">==</span> Constants<span class="token punctuation">.</span>REQUEST_QZONE_SHARE <span class="token operator">||</span>
                    requestCode <span class="token operator">==</span> Constants<span class="token punctuation">.</span>REQUEST_APPBAR<span class="token punctuation">)</span> <span class="token punctuation">{</span>
                Tencent<span class="token punctuation">.</span><span class="token function">onActivityResultData</span><span class="token punctuation">(</span>requestCode<span class="token punctuation">,</span> resultCode<span class="token punctuation">,</span> data<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
                <span class="token keyword">return</span> <span class="token boolean">true</span><span class="token punctuation">;</span>
            <span class="token punctuation">}</span>
            <span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span>

</code></pre>
</div>
</body>

</html>
