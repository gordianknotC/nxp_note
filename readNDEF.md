<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>readNDEF</title>
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
  <div class="stackedit__html"><h1 id="how-readndef-implements">How ReadNDEF Implements</h1>
<h2 id="in-ntag_i2c_command">in Ntag_I2C_Command</h2>
<h3 id="readndef">readNDEF</h3>
<blockquote>
<p>Read a NDEF Message from the tag - not an official NFC Forum NDEF detection routine.</p>
</blockquote>
<h4 id="describe-reading-process-in-brief">describe reading process in brief</h4>
<ul>
<li>
<p>params (none)</p>
</li>
<li>
<p>get size of <strong>TLV</strong> and <strong>NDEF</strong></p>
<ul>
<li>get <strong>TLV</strong> by <strong>readEEPROM</strong>( P1, P2 )
<ul>
<li>P1: Register.<strong>User_memory_Begin</strong></li>
<li>P2: Register.<strong>User_memory_Begin</strong> + <strong>3</strong></li>
</ul>
</li>
<li>read byte data using <strong>readEEPROM</strong>( P1, P2 )
<ul>
<li>P1: Register.<strong>User_memory_Begin</strong></li>
<li>P2: Register.<strong>User_memory_Begin</strong> + tlvPlusNdef</li>
</ul>
</li>
<li>feed byte data into <strong>NdefMessage</strong> as return value</li>
</ul>
<h5 id="about-tlv-type-length-value">about TLV (Type Length-Value)</h5>
<ul>
<li>A kind of data exchange format
<ul>
<li>T: Type information</li>
<li>L: Length information about playload data</li>
<li>V: playload data</li>
</ul>
</li>
</ul>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>getRegister_Settings</li>
<li>NDEFReadTask / doInBackground</li>
<li>EEPROMSpeedtestTask / doInBackground</li>
</ul>
</li>
</ul>
<h5 id="example">example</h5>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">//read records of NdefMessage into String</span>
	NdefMessage message <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">readNDEF</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	String NDEFText <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">String</span><span class="token punctuation">(</span>message<span class="token punctuation">.</span><span class="token function">getRecords</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">.</span><span class="token function">getPayload</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span><span class="token string">"US-ASCII"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token comment">// Execute Read test  </span>
	readTime <span class="token operator">=</span> System<span class="token punctuation">.</span><span class="token function">currentTimeMillis</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	NdefMessage message <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">readNDEF</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	readTime <span class="token operator">=</span> System<span class="token punctuation">.</span><span class="token function">currentTimeMillis</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">-</span> readTime<span class="token punctuation">;</span>
</code></pre>
</li>
</ul>
<p><strong>Main Code</strong></p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> NdefMessage <span class="token function">readNDEF</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">int</span> ndefsize<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> tlvsize<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> tlvPlusNdef<span class="token punctuation">;</span>  

		<span class="token comment">// get TLV  </span>
		<span class="token comment">// note: [TLV] seems like a lock control  </span>
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> tlv <span class="token operator">=</span> <span class="token function">readEEPROM</span><span class="token punctuation">(</span>
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>  
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// checking TLV - maybe there are other TLVs on the tag  </span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">!=</span> <span class="token number">0x03</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">FormatException</span><span class="token punctuation">(</span><span class="token string">"Format on Tag not supported"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">!=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			ndefsize <span class="token operator">=</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">&amp;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			tlvsize <span class="token operator">=</span> <span class="token number">2</span><span class="token punctuation">;</span>  
			tlvPlusNdef <span class="token operator">=</span> tlvsize <span class="token operator">+</span> ndefsize<span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			ndefsize <span class="token operator">=</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">&amp;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			ndefsize <span class="token operator">|=</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">&lt;&lt;</span> <span class="token number">8</span><span class="token punctuation">)</span> <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			tlvsize <span class="token operator">=</span> <span class="token number">4</span><span class="token punctuation">;</span>  
			tlvPlusNdef <span class="token operator">=</span> tlvsize <span class="token operator">+</span> ndefsize<span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token comment">// Read NDEF Message  </span>
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data <span class="token operator">=</span> <span class="token function">readEEPROM</span><span class="token punctuation">(</span>
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>  
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token punctuation">(</span>tlvPlusNdef <span class="token operator">/</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// delete TLV  </span>
		data <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> tlvsize<span class="token punctuation">,</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token comment">// delete end of String which is not part of the NDEF Message  </span>
		data <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOf</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> ndefsize<span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// get the String out of the Message  </span>
		NdefMessage message <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">NdefMessage</span><span class="token punctuation">(</span>data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> message<span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
</code></pre>
<h2 id="in-minimalntag_i2c_command">in MinimalNtag_I2C_Command</h2>
<h3 id="readndef-1">readNDEF</h3>
<blockquote>
<p>Read a NDEF Message from the tag - not an official NFC Forum NDEF detection routine.</p>
</blockquote>
<ul>
<li>
<p>params (none)</p>
</li>
<li>
<p>get size of <strong>TLV</strong> and <strong>NDEF</strong></p>
<ul>
<li>get <strong>TLV</strong> by <strong>readEEPROM</strong>( P1, P2 )
<ul>
<li>P1: Register.<strong>User_memory_Begin</strong></li>
<li>P2: Register.<strong>User_memory_Begin</strong> + <strong>3</strong></li>
</ul>
</li>
<li>read byte data using <strong>readEEPROM</strong>( P1, P2 )
<ul>
<li>P1: Register.<strong>User_memory_Begin</strong></li>
<li>P2: Register.<strong>User_memory_Begin</strong> + tlvPlusNdef / <strong>4</strong></li>
</ul>
</li>
<li>feed byte data into <strong>NdefMessage</strong> as return value</li>
</ul>
<h5 id="about-tlv-type-length-value-1">about TLV (Type Length-Value)</h5>
<ul>
<li>A kind of data exchange format
<ul>
<li>T: Type information</li>
<li>L: Length information about playload data</li>
<li>V: playload data</li>
</ul>
</li>
</ul>
<h5 id="referenced-in-1">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>getRegister_Settings</li>
<li>NDEFReadTask / doInBackground</li>
<li>EEPROMSpeedtestTask / doInBackground</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> NdefMessage <span class="token function">readNDEF</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">int</span> ndefsize<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> tlvsize<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> tlvPlusNdef<span class="token punctuation">;</span>  

		<span class="token comment">// get TLV  </span>
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> tlv <span class="token operator">=</span> <span class="token function">readEEPROM</span><span class="token punctuation">(</span>
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>  
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// checking TLV - maybe there are other TLVs on the tag  </span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">!=</span> <span class="token number">0x03</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">FormatException</span><span class="token punctuation">(</span><span class="token string">"Format on Tag not supported"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">if</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">!=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			ndefsize <span class="token operator">=</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">&amp;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			tlvsize <span class="token operator">=</span> <span class="token number">2</span><span class="token punctuation">;</span>  
			tlvPlusNdef <span class="token operator">=</span> tlvsize <span class="token operator">+</span> ndefsize<span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			ndefsize <span class="token operator">=</span> <span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">&amp;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			ndefsize <span class="token operator">|=</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>tlv<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">&lt;&lt;</span> <span class="token number">8</span><span class="token punctuation">)</span> <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			tlvsize <span class="token operator">=</span> <span class="token number">4</span><span class="token punctuation">;</span>  
			tlvPlusNdef <span class="token operator">=</span> tlvsize <span class="token operator">+</span> ndefsize<span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token comment">// Read NDEF Message  </span>
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data <span class="token operator">=</span> <span class="token function">readEEPROM</span><span class="token punctuation">(</span>
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>  
			Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token punctuation">(</span>tlvPlusNdef <span class="token operator">/</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// delete TLV  </span>
		data <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> tlvsize<span class="token punctuation">,</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token comment">// delete end of String which is not part of the NDEF Message  </span>
		data <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOf</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> ndefsize<span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Interpret Bytes  </span>
		NdefMessage message <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">NdefMessage</span><span class="token punctuation">(</span>data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> message<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
</div>
</body>

</html>
