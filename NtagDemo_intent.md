<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>NtagDemo_intent</title>
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
  <div class="stackedit__html"><h2 id="how-new-intent-be-processed">How New Intent be Processed</h2>
<ul>
<li>
<h5 id="in-authactivity">in AuthActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">protected</span> <span class="token keyword">void</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent nfc_intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token keyword">super</span><span class="token punctuation">.</span><span class="token function">onNewIntent</span><span class="token punctuation">(</span>nfc_intent<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token comment">// Set the pattern for vibration  </span>
		<span class="token keyword">long</span> pattern<span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">{</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">100</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>  

		<span class="token comment">// Vibrate on new Intent  </span>
		Vibrator vibrator <span class="token operator">=</span> <span class="token punctuation">(</span>Vibrator<span class="token punctuation">)</span> <span class="token function">getSystemService</span><span class="token punctuation">(</span>Context<span class="token punctuation">.</span>VIBRATOR_SERVICE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		vibrator<span class="token punctuation">.</span><span class="token function">vibrate</span><span class="token punctuation">(</span>pattern<span class="token punctuation">,</span> <span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Get the tag and start the demo  </span>
		Tag tag <span class="token operator">=</span> nfc_intent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> 
			<span class="token keyword">this</span><span class="token punctuation">,</span> MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">ObtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// This authentication is added in order to avoid authentication problems with old NFC Controllers  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			demo<span class="token punctuation">.</span><span class="token function">Auth</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
				 AuthStatus<span class="token punctuation">.</span>Protected_RW<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token comment">// Set the Auth Status on the screen  </span>
		<span class="token function">updateAuthStatus</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="mainactivity">MainActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">protected</span> <span class="token keyword">void</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent nfc_intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	  <span class="token keyword">super</span><span class="token punctuation">.</span><span class="token function">onNewIntent</span><span class="token punctuation">(</span>nfc_intent<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  <span class="token comment">// Set the pattern for vibration  </span>
	  <span class="token keyword">long</span> pattern<span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">{</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">100</span> <span class="token punctuation">}</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Set the initial auth parameters  </span>
	  mAuthStatus <span class="token operator">=</span> AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  mPassword <span class="token operator">=</span> null<span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Vibrate on new Intent  </span>
	  Vibrator vibrator <span class="token operator">=</span> <span class="token punctuation">(</span>Vibrator<span class="token punctuation">)</span> <span class="token function">getSystemService</span><span class="token punctuation">(</span>Context<span class="token punctuation">.</span>VIBRATOR_SERVICE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  vibrator<span class="token punctuation">.</span><span class="token function">vibrate</span><span class="token punctuation">(</span>pattern<span class="token punctuation">,</span> <span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  <span class="token function">doProcess</span><span class="token punctuation">(</span>nfc_intent<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>

	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">doProcess</span><span class="token punctuation">(</span>Intent nfc_intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// m interpreted as mounted</span>
		mIntent <span class="token operator">=</span> nfc_intent<span class="token punctuation">;</span>  
		Tag tag <span class="token operator">=</span> nfc_intent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> mPassword<span class="token punctuation">,</span> mAuthStatus<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token comment">// Retrieve Auth Status before doing any operation  </span>
			mAuthStatus <span class="token operator">=</span> <span class="token function">obtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			String currTab <span class="token operator">=</span> mTabHost<span class="token punctuation">.</span><span class="token function">getCurrentTabTag</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token function">launchDemo</span><span class="token punctuation">(</span>currTab<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
	<span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">void</span> <span class="token function">setNfcIntent</span><span class="token punctuation">(</span>Intent intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	   mIntent <span class="token operator">=</span> intent<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="registerconfigactivity">RegisterConfigActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">protected</span> <span class="token keyword">void</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent nfc_intent<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// Set the initial auth parameters  </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		MainActivity<span class="token punctuation">.</span><span class="token function">setPassword</span><span class="token punctuation">(</span>null<span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Set the Write protection check  </span>
		isWriteProtected <span class="token operator">=</span> <span class="token boolean">false</span><span class="token punctuation">;</span>  

		<span class="token comment">// Store the intent information  </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setNfcIntent</span><span class="token punctuation">(</span>nfc_intent<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">final</span> Tag tag <span class="token operator">=</span> nfc_intent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">true</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="registersessionactivity">RegisterSessionActivity</h5>
</li>
<li>
<h5 id="in-flashmemoryactivity">in FlashMemoryActivity</h5>
</li>
<li>
<h5 id="versionactivity">VersionActivity</h5>
</li>
<li>
<h5 id="readmemoryactivity">ReadMemoryActivity</h5>
</li>
<li>
<h5 id="resetmemeoryactivity">ResetMemeoryActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">protected</span> <span class="token keyword">void</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent nfcIntent<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// Set the initial auth parameters  </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		MainActivity<span class="token punctuation">.</span><span class="token function">setPassword</span><span class="token punctuation">(</span>null<span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Store the intent information  </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setNfcIntent</span><span class="token punctuation">(</span>nfcIntent<span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Complete the task in a new thread in order to be able to show the dialog  </span>
		Tag tag <span class="token operator">=</span> nfcIntent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">true</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
</div>
</body>

</html>
