<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>NtagDemo_</title>
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
  <div class="stackedit__html"><h1 id="ntag_i2c_demo">Ntag_I2C_Demo</h1>
<h3 id="properties-and-constructor">Properties and Constructor</h3>
<h2 id="constructor">Constructor</h2>
<h4 id="describe-process-in-brief">describe process in brief</h4>
<ul>
<li>params
<ul>
<li>tag <strong>Tag</strong> with which the Demos should be performed</li>
<li>main <strong>MainActivity</strong></li>
<li>passwd <strong>byte[]</strong></li>
<li>authStatus <strong>int</strong></li>
</ul>
</li>
<li>return null while tag is null</li>
<li>initialize reader by <strong>I2C_Enabled_Commands</strong>.get(tag)</li>
<li>connect reader</li>
<li>get product info by reader.<strong>getProduct</strong>()</li>
</ul>
<p><strong>Main Code</strong></p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">public</span> <span class="token function">Ntag_I2C_Demo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">,</span> <span class="token keyword">final</span> Activity main<span class="token punctuation">,</span> <span class="token keyword">final</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> passwd<span class="token punctuation">,</span> <span class="token keyword">final</span> <span class="token keyword">int</span> authStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token keyword">try</span> <span class="token punctuation">{</span>
			<span class="token keyword">if</span> <span class="token punctuation">(</span>tag <span class="token operator">==</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
				<span class="token keyword">this</span><span class="token punctuation">.</span>main <span class="token operator">=</span> null<span class="token punctuation">;</span>
				<span class="token keyword">this</span><span class="token punctuation">.</span>tag <span class="token operator">=</span> null<span class="token punctuation">;</span>
				<span class="token keyword">return</span><span class="token punctuation">;</span>
			<span class="token punctuation">}</span>
			<span class="token keyword">this</span><span class="token punctuation">.</span>main <span class="token operator">=</span> main<span class="token punctuation">;</span>
			<span class="token keyword">this</span><span class="token punctuation">.</span>tag <span class="token operator">=</span> tag<span class="token punctuation">;</span>

			reader <span class="token operator">=</span> I2C_Enabled_Commands<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>

			<span class="token keyword">if</span> <span class="token punctuation">(</span>reader <span class="token operator">==</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
				String message <span class="token operator">=</span> <span class="token string">"The Tag could not be identified or this NFC device does not "</span>
						<span class="token operator">+</span> <span class="token string">"support the NFC Forum commands needed to access this tag"</span><span class="token punctuation">;</span>
				String title <span class="token operator">=</span> <span class="token string">"Communication failed"</span><span class="token punctuation">;</span>
				<span class="token function">showAlert</span><span class="token punctuation">(</span>message<span class="token punctuation">,</span> title<span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
				reader<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token punctuation">}</span>

			Ntag_Get_Version<span class="token punctuation">.</span>Prod prod <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

			<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">!</span>prod<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span>Ntag_Get_Version<span class="token punctuation">.</span>Prod<span class="token punctuation">.</span>Unknown<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
				<span class="token keyword">if</span> <span class="token punctuation">(</span>prod<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span>Ntag_Get_Version<span class="token punctuation">.</span>Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus<span class="token punctuation">)</span>
			     <span class="token operator">||</span> prod<span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span>Ntag_Get_Version<span class="token punctuation">.</span>Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
					<span class="token comment">// Auth status gets lost after resetting the demo when we</span>
					<span class="token comment">// obtain the product we are dealing with</span>
					<span class="token keyword">if</span><span class="token punctuation">(</span>authStatus <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
						reader<span class="token punctuation">.</span><span class="token function">authenticatePlus</span><span class="token punctuation">(</span>passwd<span class="token punctuation">)</span><span class="token punctuation">;</span>
					<span class="token punctuation">}</span>
				<span class="token punctuation">}</span>
			<span class="token punctuation">}</span>
		<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
			e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="isconnected">isConnected</h3>
<p>Checks if the tag is still connected based on the <strong>previously</strong> detected reader. Which means we already know what kind of reader has been detected.</p>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>MainActivity
<ul>
<li>onCreate -&gt; setOnTabChangedListener</li>
<li>launchNdefDemo</li>
</ul>
</li>
<li>Ntag | MinimalNtag
<ul>
<li>isConnected</li>
</ul>
</li>
<li>SpeedTestFragment
<ul>
<li>StartEEPROMSpeedTest</li>
<li>StartSRAMSpeedTest</li>
</ul>
</li>
<li>UI (RegisterConfigActivity.onClick)</li>
</ul>
<h5 id="example-in-mainactivity.oncreate">example in MainActivity.onCreate</h5>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">if</span> <span class="token punctuation">(</span>tabId<span class="token punctuation">.</span><span class="token function">equalsIgnoreCase</span><span class="token punctuation">(</span><span class="token string">"leds"</span><span class="token punctuation">)</span> <span class="token operator">&amp;&amp;</span> demo<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	  <span class="token function">launchDemo</span><span class="token punctuation">(</span>tabId<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h5 id="example-in-startxspeedtest">example in StartXSpeedTest</h5>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">if</span> <span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">&amp;&amp;</span> MainActivity<span class="token punctuation">.</span>demo<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>perform some task
</code></pre>
<p><strong>Main Code</strong></p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">boolean</span> <span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token comment">// reader here indicates Ncfa ncfa.isConnected</span>
	<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<hr>
<hr>
<h3 id="istagpresent">isTagPresent</h3>
<p>Checks if the tag is still connected based on the tag. Which means we don’t know what kind of reader has been resolved.</p>
<h4 id="describe-process-in-brief-1">describe process in brief</h4>
<ul>
<li>
<p>params</p>
<ul>
<li><strong>Tag</strong> tag</li>
</ul>
</li>
<li>
<p>get Ndef by Ndef.get(tag) then perform following detection.</p>
</li>
<li>
<p>detect if it’s connected</p>
<ul>
<li>ndef.isConnected()</li>
<li>nfca.isConnected()</li>
<li>nfcb.isConnected()</li>
<li>nfcf.isConnected()</li>
<li>nfcv.isConnected()</li>
</ul>
</li>
<li>
<p>close ndef and return its result ( isConnected)</p>
<h5 id="referenced-in-1">referenced in</h5>
<ul>
<li>RegisterSessionActivity</li>
<li>ResetMemoryActivity</li>
<li>RegisterConfigActivity</li>
<li>AuthActivity</li>
</ul>
<h5 id="example">example:</h5>
<pre class=" language-java"><code class="prism  language-java"><span class="token comment">// Capture intent to check whether the operation should be automatically launch or not  </span>
Tag tag <span class="token operator">=</span> <span class="token function">getIntent</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getParcelableExtra</span><span class="token punctuation">(</span>NfcAdapter<span class="token punctuation">.</span>EXTRA_TAG<span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token keyword">if</span><span class="token punctuation">(</span>tag <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> Ntag_I2C_Demo<span class="token punctuation">.</span><span class="token function">isTagPresent</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	<span class="token function">startDemo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token boolean">false</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
</li>
</ul>
<p><strong>Main Code</strong></p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">boolean</span> <span class="token function">isTagPresent</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">final</span> Ndef ndef <span class="token operator">=</span> Ndef<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span>ndef <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> <span class="token operator">!</span>ndef<span class="token punctuation">.</span><span class="token function">getType</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">equals</span><span class="token punctuation">(</span><span class="token string">"android.ndef.unknown"</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token keyword">try</span> <span class="token punctuation">{</span>
			ndef<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token keyword">final</span> <span class="token keyword">boolean</span> isConnected <span class="token operator">=</span> ndef<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			ndef<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token keyword">return</span> isConnected<span class="token punctuation">;</span>
		<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">final</span> IOException e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
			e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
	<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
		<span class="token keyword">final</span> NfcA nfca <span class="token operator">=</span> NfcA<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>nfca <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
			<span class="token keyword">try</span> <span class="token punctuation">{</span>
				nfca<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
				<span class="token keyword">final</span> <span class="token keyword">boolean</span> isConnected <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
				nfca<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

				<span class="token keyword">return</span> isConnected<span class="token punctuation">;</span>
			<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">final</span> IOException e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
				e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
				<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
			<span class="token punctuation">}</span>
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
			<span class="token keyword">final</span> NfcB nfcb <span class="token operator">=</span> NfcB<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>
			<span class="token keyword">if</span> <span class="token punctuation">(</span>nfcb <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
				<span class="token keyword">try</span> <span class="token punctuation">{</span>
					nfcb<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
					<span class="token keyword">final</span> <span class="token keyword">boolean</span> isConnected <span class="token operator">=</span> nfcb<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
					nfcb<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
					<span class="token keyword">return</span> isConnected<span class="token punctuation">;</span>
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">final</span> IOException e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
					e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
					<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
				<span class="token punctuation">}</span>
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
				<span class="token keyword">final</span> NfcF nfcf <span class="token operator">=</span> NfcF<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>
				<span class="token keyword">if</span> <span class="token punctuation">(</span>nfcf <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
					<span class="token keyword">try</span> <span class="token punctuation">{</span>
						nfcf<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
						<span class="token keyword">final</span> <span class="token keyword">boolean</span> isConnected <span class="token operator">=</span> nfcf<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
						nfcf<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
						<span class="token keyword">return</span> isConnected<span class="token punctuation">;</span>
					<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">final</span> IOException e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
						e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
						<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
					<span class="token punctuation">}</span>
				<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
					<span class="token keyword">final</span> NfcV nfcv <span class="token operator">=</span> NfcV<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>
					<span class="token keyword">if</span> <span class="token punctuation">(</span>nfcv <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
						<span class="token keyword">try</span> <span class="token punctuation">{</span>
							nfcv<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
							<span class="token keyword">final</span> <span class="token keyword">boolean</span> isConnected <span class="token operator">=</span> nfcv<span class="token punctuation">.</span><span class="token function">isConnected</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
							nfcv<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
							<span class="token keyword">return</span> isConnected<span class="token punctuation">;</span>
						<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">final</span> IOException e<span class="token punctuation">)</span> <span class="token punctuation">{</span>
							e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
							<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
						<span class="token punctuation">}</span>
					<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
						<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
					<span class="token punctuation">}</span>
				<span class="token punctuation">}</span>
			<span class="token punctuation">}</span>
		<span class="token punctuation">}</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="isready">isReady</h3>
<p>Checks if the demo is ready to be executed.</p>
<h4 id="describe-process-in-brief-2">describe process in brief</h4>
<ul>
<li>tag != null</li>
<li>reader != null</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">public</span> <span class="token keyword">boolean</span> <span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tag <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> reader <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>
			<span class="token keyword">return</span> <span class="token boolean">true</span><span class="token punctuation">;</span>
		<span class="token punctuation">}</span>
		<span class="token keyword">return</span> <span class="token boolean">false</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<hr>
<h3 id="getproduct">getProduct</h3>
<p>Get the product from detected reader.</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> Prod <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException <span class="token punctuation">{</span>
	<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>

<span class="token keyword">class</span> <span class="token class-name">MyFlutterActivity</span> <span class="token operator">:</span> <span class="token function">FlutterActivity</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  override fun <span class="token function">onCreate</span><span class="token punctuation">(</span>savedInstanceState<span class="token operator">:</span> Bundle<span class="token operator">?</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
    <span class="token keyword">super</span><span class="token punctuation">.</span><span class="token function">onCreate</span><span class="token punctuation">(</span>savedInstanceState<span class="token punctuation">)</span>  
    GeneratedPluginRegistrant<span class="token punctuation">.</span><span class="token function">registerWith</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span>  
  <span class="token punctuation">}</span>  
<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="setcurrentboardversion">setCurrentBoardVersion</h3>
<p>Set the current board version.</p>
<h4 id="describe-process-in-brief-3">describe process in brief</h4>
<ul>
<li>UI - onBoardVersionSetted<pre class=" language-java"><code class="prism  language-java"> <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onBoardVersionSetted</span><span class="token punctuation">(</span>String ver<span class="token punctuation">,</span> String fwver<span class="token punctuation">)</span><span class="token punctuation">{</span>  
  <span class="token comment">//todo: link to ui  </span>
  VersionInfoActivity<span class="token punctuation">.</span><span class="token function">setBoardVersion</span><span class="token punctuation">(</span>ver<span class="token punctuation">)</span><span class="token punctuation">;</span>  
  VersionInfoActivity<span class="token punctuation">.</span><span class="token function">setBoardFWVersion</span><span class="token punctuation">(</span>fwver<span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
</li>
</ul>
<h5 id="referenced-in-2">referenced in</h5>
<ul>
<li>VersionInfoActivity
<ul>
<li>onActivityResult</li>
<li>startDemo</li>
</ul>
</li>
</ul>
<hr>
<h3 id="resettagmemory">resetTagMemory</h3>
<p>Resets the tag to its delivery values (including config registers).</p>
<h4 id="describe-process-in-brief-4">describe process in brief</h4>
<ul>
<li>params
<ul>
<li>@return <strong>int</strong> indicating number of bytes written</li>
</ul>
<h5 id="referenced-in-3">referenced in</h5>
<ul>
<li>ResetMemoryActivity
<ul>
<li>doInBackground</li>
</ul>
</li>
</ul>
<h5 id="example---statistic-time-to-reset-memory">example - statistic time to reset memory</h5>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">long</span> regTimeOutStart <span class="token operator">=</span> System<span class="token punctuation">.</span><span class="token function">currentTimeMillis</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token comment">// Reset Tag demo will return the number of bytes written  </span>
<span class="token keyword">int</span> bytes <span class="token operator">=</span> demo<span class="token punctuation">.</span><span class="token function">resetTagMemory</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token comment">// Memory erase time statistics  </span>
timeToResetMemory <span class="token operator">=</span> System<span class="token punctuation">.</span><span class="token function">currentTimeMillis</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">-</span> regTimeOutStart<span class="token punctuation">;</span>  
</code></pre>
</li>
</ul>
<hr>
<h3 id="getregister_settings">getRegister_Settings</h3>
<p>Builds a String array for the Registers.</p>
<h4 id="describe-process-in-brief-5">describe process in brief</h4>
<ul>
<li>params
<ul>
<li>register <strong>[]byte</strong> of the Registers</li>
<li>@return <strong>Ntag_I2C_Registers</strong> ( a String Array )</li>
</ul>
</li>
</ul>
<h5 id="referenced-in-4">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>readSessionRegisters</li>
<li>readWriteConfigRegister</li>
</ul>
</li>
</ul>
<h5 id="example-1">example</h5>
</div>
</body>

</html>
