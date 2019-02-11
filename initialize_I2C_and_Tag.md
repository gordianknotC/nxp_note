<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>initialize_I2C_and_Tag</title>
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
  <div class="stackedit__html"><h1 id="how-ntag-i2c-demo--tag-be-initialized">How Ntag I2C Demo / Tag be Initialized</h1>
<h4 id="ntag_i2c_demo">Ntag_I2C_demo</h4>
<p>Ntag_I2C_demo constructor</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token function">Ntag_I2C_Demo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">,</span> <span class="token keyword">final</span> Activity main<span class="token punctuation">,</span> <span class="token keyword">final</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> passwd<span class="token punctuation">,</span> <span class="token keyword">final</span> <span class="token keyword">int</span> authStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token punctuation">}</span>
</code></pre>
<h5 id="in-authactivity">in AuthActivity</h5>
<ul>
<li>
<p>onCreate</p>
<pre class=" language-java"><code class="prism  language-java">mTag <span class="token operator">=</span> <span class="token function">getIntent</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span> 
<span class="token keyword">if</span><span class="token punctuation">(</span>mTag <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> Ntag_I2C_Demo<span class="token punctuation">.</span><span class="token function">isTagPresent</span><span class="token punctuation">(</span>mTag<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> 
	demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span> 
		mTag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span>             
		MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>   
		MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span> 
</code></pre>
</li>
<li>
<p>onNewIntent</p>
<pre class=" language-java"><code class="prism  language-java">	Tag tag <span class="token operator">=</span> nfc_intent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>
	demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span> tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span>             
		MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>   
		MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
</code></pre>
</li>
<li>
<p>startDemo</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">)</span> <span class="token punctuation">{</span>      
		<span class="token comment">// This authentication is added in order to avoid authentication problems with old NFC Controllers    </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>    
			demo<span class="token punctuation">.</span><span class="token function">Auth</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> AuthStatus<span class="token punctuation">.</span>Protected_RW<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
		<span class="token punctuation">}</span>    
		<span class="token keyword">if</span><span class="token punctuation">(</span>demo <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>    
			<span class="token comment">// Launch the thread    </span>
			task <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">authTask</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
			task<span class="token punctuation">.</span><span class="token function">execute</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
		<span class="token punctuation">}</span>    
	<span class="token punctuation">}</span>  
</code></pre>
</li>
</ul>
<h5 id="in-flashmemoryactivity">in FlashMemoryActivity</h5>
<ul>
<li>
<p>onNewIntent</p>
<pre class=" language-java"><code class="prism  language-java">	Tag tag <span class="token operator">=</span> nfcIntent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">true</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
</code></pre>
</li>
<li>
<p>startDemo</p>
<pre class=" language-java"><code class="prism  language-java">	demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span> tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span>             
		MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>   
		MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token keyword">return</span><span class="token punctuation">;</span> 
</code></pre>
</li>
</ul>
<h5 id="mainactivity">MainActivity</h5>
<ul>
<li>
<p>onCreate</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">// Initialize the demo in order to handle tab change events </span>
	demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>null<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> null<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
	mAdapter <span class="token operator">=</span> NfcAdapter<span class="token punctuation">.</span><span class="token function">getDefaultAdapter</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
	<span class="token function">setNfcForeground</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
	<span class="token function">checkNFC</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
</code></pre>
<ul>
<li>onNewIntent</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">protected</span> <span class="token keyword">void</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent nfc_intent<span class="token punctuation">)</span> <span class="token punctuation">{</span> 
		<span class="token keyword">super</span><span class="token punctuation">.</span><span class="token function">onNewIntent</span><span class="token punctuation">(</span>nfc_intent<span class="token punctuation">)</span><span class="token punctuation">;</span> 
		<span class="token comment">// Set the pattern for vibration long pattern[] = { 0, 100 };            </span>
		<span class="token comment">// Set the initial auth parameters  </span>
		mAuthStatus <span class="token operator">=</span> AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
		mPassword <span class="token operator">=</span> null<span class="token punctuation">;</span>            
		<span class="token comment">// Vibrate on new Intent  </span>
		Vibrator vibrator <span class="token operator">=</span> <span class="token punctuation">(</span>Vibrator<span class="token punctuation">)</span> <span class="token function">getSystemService</span><span class="token punctuation">(</span>Context<span class="token punctuation">.</span>VIBRATOR_SERVICE<span class="token punctuation">)</span><span class="token punctuation">;</span> 		
		vibrator<span class="token punctuation">.</span><span class="token function">vibrate</span><span class="token punctuation">(</span>pattern<span class="token punctuation">,</span> <span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
		<span class="token function">doProcess</span><span class="token punctuation">(</span>nfc_intent<span class="token punctuation">)</span><span class="token punctuation">;</span> 
	<span class="token punctuation">}</span> 
</code></pre>
<ul>
<li>doProcess</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">doProcess</span><span class="token punctuation">(</span>Intent nfc_intent<span class="token punctuation">)</span> <span class="token punctuation">{</span> 
		mIntent <span class="token operator">=</span> nfc_intent<span class="token punctuation">;</span> 
		Tag tag <span class="token operator">=</span> nfc_intent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span> 
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> mPassword<span class="token punctuation">,</span> mAuthStatus<span class="token punctuation">)</span><span class="token punctuation">;</span> 
		<span class="token keyword">if</span> <span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> 
			<span class="token comment">// Retrieve Auth Status before doing any operation </span>
			mAuthStatus <span class="token operator">=</span> <span class="token function">obtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
			String currTab <span class="token operator">=</span> mTabHost<span class="token punctuation">.</span><span class="token function">getCurrentTabTag</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
			<span class="token function">launchDemo</span><span class="token punctuation">(</span>currTab<span class="token punctuation">)</span><span class="token punctuation">;</span> 
		<span class="token punctuation">}</span> 
	<span class="token punctuation">}</span> 
</code></pre>
</li>
</ul>
<h5 id="registerconfigactivity">RegisterConfigActivity</h5>
<ul>
<li>
<p>onCreate</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">// Capture intent to check whether the operation should be automatically launch or not    </span>
	Tag tag <span class="token operator">=</span> <span class="token function">getIntent</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token keyword">if</span><span class="token punctuation">(</span>tag <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> Ntag_I2C_Demo<span class="token punctuation">.</span><span class="token function">isTagPresent</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>    
		<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">false</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token punctuation">}</span>    

	<span class="token comment">// Add Foreground dispatcher    </span>
	mAdapter <span class="token operator">=</span> NfcAdapter<span class="token punctuation">.</span><span class="token function">getDefaultAdapter</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
	pendingIntent <span class="token operator">=</span> PendingIntent<span class="token punctuation">.</span><span class="token function">getActivity</span><span class="token punctuation">(</span>  
		<span class="token keyword">this</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span>      
		<span class="token keyword">new</span> <span class="token class-name">Intent</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">,</span> <span class="token function">getClass</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
	<span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">addFlags</span><span class="token punctuation">(</span>Intent<span class="token punctuation">.</span>FLAG_ACTIVITY_SINGLE_TOP<span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token keyword">return</span><span class="token punctuation">;</span> <span class="token comment">// end onCreate  </span>
</code></pre>
</li>
<li>
<p>startDemo</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span><span class="token keyword">final</span> Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>      
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span>   
			MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span>    
			<span class="token keyword">return</span><span class="token punctuation">;</span>  
</code></pre>
</li>
</ul>
<h5 id="registersessionactivity">RegisterSessionActivity</h5>
<ul>
<li>
<p>onCreate</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">// Capture intent to check whether the operation should be automatically launch or not    </span>
	Tag tag <span class="token operator">=</span> <span class="token function">getIntent</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token keyword">if</span><span class="token punctuation">(</span>tag <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> Ntag_I2C_Demo<span class="token punctuation">.</span><span class="token function">isTagPresent</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>    
		<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">false</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token punctuation">}</span>  
</code></pre>
</li>
<li>
<p>startDemo</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span><span class="token keyword">final</span> Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>      
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span>   
		MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>   
		MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>    
			<span class="token keyword">return</span><span class="token punctuation">;</span>    
		<span class="token punctuation">}</span>  
</code></pre>
</li>
<li>
<p>onNewIntent</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">protected</span> <span class="token keyword">void</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent nfcIntent<span class="token punctuation">)</span> <span class="token punctuation">{</span>      
		<span class="token comment">// Set the initial auth parameters    </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
		MainActivity<span class="token punctuation">.</span><span class="token function">setPassword</span><span class="token punctuation">(</span>null<span class="token punctuation">)</span><span class="token punctuation">;</span>    

		<span class="token comment">// Store the intent information    </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setNfcIntent</span><span class="token punctuation">(</span>nfcIntent<span class="token punctuation">)</span><span class="token punctuation">;</span>    
		Tag tag <span class="token operator">=</span> nfcIntent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>    
		<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">true</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token punctuation">}</span>  
</code></pre>
</li>
</ul>
<h5 id="readmemoryactivity">ReadMemoryActivity</h5>
<ul>
<li>
<p>onNewIntent</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">protected</span> <span class="token keyword">void</span> <span class="token function">onNewIntent</span><span class="token punctuation">(</span>Intent nfcIntent<span class="token punctuation">)</span> <span class="token punctuation">{</span>      
		<span class="token comment">// Set the initial auth parameters    </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
		MainActivity<span class="token punctuation">.</span><span class="token function">setPassword</span><span class="token punctuation">(</span>null<span class="token punctuation">)</span><span class="token punctuation">;</span>    

		<span class="token comment">// Store the intent information    </span>
		MainActivity<span class="token punctuation">.</span><span class="token function">setNfcIntent</span><span class="token punctuation">(</span>nfcIntent<span class="token punctuation">)</span><span class="token punctuation">;</span>    

		Tag tag <span class="token operator">=</span> nfcIntent<span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>    
		<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">true</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token punctuation">}</span>  
</code></pre>
<ul>
<li>startDemo</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>       
		<span class="token comment">// Complete the task in a new thread in order to be able to show the dialog    </span>
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>  
			tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span>        
			MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>   
			MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>    
			<span class="token keyword">return</span><span class="token punctuation">;</span>    
		<span class="token punctuation">}</span>    
	<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>  
<span class="token punctuation">}</span> 
</code></pre>
</li>
<li>
<h5 id="versionactivity">VersionActivity</h5>
<p>the same as aboves</p>
</li>
</ul>
<hr>
<h2 id="handling-null-tag">Handling null Tag</h2>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">// within onCreate...</span>
	Tag tag <span class="token operator">=</span> <span class="token function">getIntent</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token keyword">if</span><span class="token punctuation">(</span>tag <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> Ntag_I2C_Demo<span class="token punctuation">.</span><span class="token function">isTagPresent</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>    
		<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">false</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    
	<span class="token punctuation">}</span>  
</code></pre>
<hr>
<h2 id="initialize-ntag_ic2_command">Initialize Ntag_IC2_Command</h2>
<p><strong>Ntag_IC2_Command</strong> is initialized on only one parameter. Unlike <strong>MinimalNtag_I2C_Command</strong> required two parameters, which is <strong>Tag</strong> tag and <strong>Prod</strong> tagType, since tagType in <strong>Ntag_I2C_Command</strong> can be fetched by <strong>Ntag_Get_Version</strong>.</p>
<ol>
<li><strong>Tag</strong> tag</li>
</ol>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">static</span> I2C_Enabled_Commands <span class="token function">get</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> InterruptedException <span class="token punctuation">{</span>  
	<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> answer<span class="token punctuation">;</span>  
	<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	NfcA nfca <span class="token operator">=</span> NfcA<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	Prod prod<span class="token punctuation">;</span>  

	<span class="token comment">// Check for support of setTimout to be able to send an efficient  </span>
	<span class="token comment">// sector_select - select minimal implementation if not supported  nfca.setTimeout(20);  </span>
	<span class="token comment">// check for timeout  </span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span>nfca<span class="token punctuation">.</span><span class="token function">getTimeout</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">&lt;</span> <span class="token number">50</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// check if GetVersion is supported  </span>
		<span class="token keyword">try</span> <span class="token punctuation">{</span>  
			nfca<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
			command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x60</span><span class="token punctuation">;</span> <span class="token comment">// GET_VERSION  </span>
			answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			prod <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Ntag_Get_Version</span><span class="token punctuation">(</span>answer<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">Get_Product</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			nfca<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span>prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k  
			<span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_T <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_T  
			<span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_V <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_V  
			<span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			nfca<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

			<span class="token comment">// check if sector select is supported  </span>
			<span class="token keyword">try</span> <span class="token punctuation">{</span>  
				nfca<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
				command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xC2</span><span class="token punctuation">;</span> <span class="token comment">//SECTOR_SELECT  </span>
				command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
				answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
				nfca<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>  

			<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e2<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				nfca<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>

</code></pre>
<hr>
<h2 id="initialize-minimalntag_ic2_command">Initialize MinimalNtag_IC2_Command</h2>
<p><strong>MinimalNtag_IC2_Command</strong> is initialized by two parameters</p>
<ol>
<li><strong>Tag</strong> tag</li>
<li><strong>Prod</strong> tagType (product)
<ul>
<li>generally speaking, tagType was fetched by Ntag_Get_Version(answer)</li>
<li>but there are some exceptions occurs in MininalNtag<br>
<strong>todo: rewrite a new Ntag_Get_Version to consider those exceptions</strong></li>
</ul>
</li>
</ol>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">//check if we can use the minimal Version  </span>
	MifareUltralight mfu <span class="token operator">=</span> MifareUltralight<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token keyword">try</span> <span class="token punctuation">{</span>  
		mfu<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x60</span><span class="token punctuation">;</span> <span class="token comment">// GET_VERSION  </span>
		answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		prod <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Ntag_Get_Version</span><span class="token punctuation">(</span>answer<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">Get_Product</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k  
		<span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_T <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_T  
		<span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_V <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_V  
		<span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus <span class="token operator">||</span> prod <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> prod<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">try</span> <span class="token punctuation">{</span>  
			mfu<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token comment">// no exception is thrown so the phone can use the mfu.readPages  </span>
			<span class="token comment">// function // also check if: // - tag is from NXP (byte 0 == 0x04) // - CC corresponds to a NTAG I2C 1K  </span>
			<span class="token keyword">if</span> <span class="token punctuation">(</span>answer<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x04</span> <span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">12</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xE1</span>  
			<span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">13</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x10</span> <span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">14</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x6D</span>  
			<span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">15</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token comment">// check if Config is readable (distinguish from NTAG216), if  </span>
				<span class="token comment">// not exception is thrown, and tag is not an // NTAG I2C 1k  answer = mfu.readPages(0xE8);  </span>

				<span class="token comment">// Try to read session registers to differentiate between standard and PLUS products  </span>
				<span class="token keyword">try</span> <span class="token punctuation">{</span>  
					answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xEC</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

					<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">4</span><span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						<span class="token keyword">if</span> <span class="token punctuation">(</span>answer<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">!=</span> <span class="token number">0x00</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
							prod <span class="token operator">=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus<span class="token punctuation">;</span>  
							<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> prod<span class="token punctuation">)</span><span class="token punctuation">;</span>  
						<span class="token punctuation">}</span>  
					<span class="token punctuation">}</span>  
					prod <span class="token operator">=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k<span class="token punctuation">;</span>  
					<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> prod<span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e2<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					e2<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					prod <span class="token operator">=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k<span class="token punctuation">;</span>  
					<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> prod<span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>answer<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x04</span> <span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">12</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xE1</span>  
				<span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">13</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x10</span> <span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">14</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xEA</span>  
				<span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">15</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token comment">// Try to read session registers to differentiate between standard and PLUS products  </span>
				<span class="token keyword">try</span> <span class="token punctuation">{</span>  
					answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xEC</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">4</span><span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						<span class="token keyword">if</span> <span class="token punctuation">(</span>answer<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">!=</span> <span class="token number">0x00</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
							prod <span class="token operator">=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">;</span>  
							<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> prod<span class="token punctuation">)</span><span class="token punctuation">;</span>  
						<span class="token punctuation">}</span>  
					<span class="token punctuation">}</span>  
					prod <span class="token operator">=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">;</span>  
					<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> prod<span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e2<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					e2<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					prod <span class="token operator">=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">;</span>  
					<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> prod<span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e1<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			e1<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>  
	<span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">MinimalNtag_I2C_Commands</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus<span class="token punctuation">)</span><span class="token punctuation">;</span>

</code></pre>
</div>
</body>

</html>
