<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>writeNDEF</title>
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
  <div class="stackedit__html"><h2 id="how-writendef-implements">How WriteNDEF Implements</h2>
<h4 id="writendef">writeNDEF</h4>
<blockquote>
<p>Write a NDEF Message from the tag - not an official NFC Forum NDEF  detection routine.</p>
</blockquote>
<h4 id="describe-writendef-process-in-brief">describe writeNDEF process in brief</h4>
<ul>
<li><strong>params</strong>
<ul>
<li><strong>NdefMessage</strong> message
<ul>
<li>NDEF message to write on the tag, can be created via <strong>createNdefMessage</strong> by a <strong>String</strong> text</li>
</ul>
</li>
<li><strong>WriteEEPROMListener</strong> listenr
<ul>
<li>A listener for implementing user responses, e.g. showing writing process to user</li>
</ul>
</li>
</ul>
</li>
<li>message written into writeNDEF was created by <strong>createNdefMessage</strong> as a <strong>NdefMessage</strong></li>
<li><strong>NdefMessage</strong> then to be transform into <strong>byte[]</strong> by <strong>createRawNdefTlv</strong>  (TLV seems like a lock controller)</li>
<li>provide a transformed <strong>byte[]</strong> to be feed into <strong>writeEEPROM</strong></li>
</ul>
<h4 id="about-tlv-type-length-value">about TLV (Type Length-Value)</h4>
<p>A kind of data exchange format<br>
T: Type information<br>
L: Length information about playload data<br>
V: playload data</p>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>NDEF</li>
<li>doInBackground</li>
<li>NDEFWrite</li>
</ul>
</li>
</ul>
<h3 id="in-ntag_i2c_command">in Ntag_I2C_Command</h3>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeNDEF</span><span class="token punctuation">(</span>NdefMessage message<span class="token punctuation">,</span> WriteEEPROMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> Ndef_message_byte <span class="token operator">=</span> <span class="token function">createRawNdefTlv</span><span class="token punctuation">(</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token function">writeEEPROM</span><span class="token punctuation">(</span>Ndef_message_byte<span class="token punctuation">,</span> listener<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>	 
</code></pre>
<hr>
<h3 id="in-minimalntag_i2c_command">in MinimalNtag_I2C_Command</h3>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeNDEF</span><span class="token punctuation">(</span>NdefMessage message<span class="token punctuation">,</span> WriteEEPROMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> ndefMessageByte <span class="token operator">=</span> <span class="token function">createRawNdefTlv</span><span class="token punctuation">(</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token function">writeEEPROM</span><span class="token punctuation">(</span>ndefMessageByte<span class="token punctuation">,</span> listener<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="writeeeprom-listener">writeEEPROM listener</h3>
<p>writeEEPROM listener has been referenced in following files</p>
<ul>
<li>Ntag_Demo / onWriteEEPROM</li>
<li>Ntag_I2C_Command / writeNDEF</li>
<li>MinimalNtag_I2C_Command / writeNDEF</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Override</span>  
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onWriteEEPROM</span><span class="token punctuation">(</span><span class="token keyword">final</span> <span class="token keyword">int</span> bytes<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	main<span class="token punctuation">.</span><span class="token function">runOnUiThread</span><span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Runnable</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token annotation punctuation">@Override</span>  
		<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">run</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			SpeedTestFragment<span class="token punctuation">.</span><span class="token function">setDatarateCallback</span><span class="token punctuation">(</span>String<span class="token punctuation">.</span><span class="token function">valueOf</span><span class="token punctuation">(</span>bytes<span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token string">" Bytes written"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="createndefmessage">CreateNDEFMessage</h3>
<h5 id="referenced-in-1">referenced in</h5>
<ul>
<li>Ntag_Demo
<ul>
<li>onPreExecute</li>
<li></li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> NdefMessage <span class="token function">createNdefMessage</span><span class="token punctuation">(</span>String text<span class="token punctuation">)</span> <span class="token keyword">throws</span> UnsupportedEncodingException <span class="token punctuation">{</span>  
		String lang <span class="token operator">=</span> <span class="token string">"en"</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> textBytes <span class="token operator">=</span> text<span class="token punctuation">.</span><span class="token function">getBytes</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> langBytes <span class="token operator">=</span> lang<span class="token punctuation">.</span><span class="token function">getBytes</span><span class="token punctuation">(</span><span class="token string">"US-ASCII"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">int</span> langLength <span class="token operator">=</span> langBytes<span class="token punctuation">.</span>length<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> textLength <span class="token operator">=</span> textBytes<span class="token punctuation">.</span>length<span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> payload <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">1</span> <span class="token operator">+</span> langLength <span class="token operator">+</span> textLength<span class="token punctuation">]</span><span class="token punctuation">;</span>  
		payload<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> langLength<span class="token punctuation">;</span>  
		System<span class="token punctuation">.</span><span class="token function">arraycopy</span><span class="token punctuation">(</span>langBytes<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> payload<span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">,</span> langLength<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		System<span class="token punctuation">.</span><span class="token function">arraycopy</span><span class="token punctuation">(</span>textBytes<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> payload<span class="token punctuation">,</span> <span class="token number">1</span> <span class="token operator">+</span> langLength<span class="token punctuation">,</span> textLength<span class="token punctuation">)</span><span class="token punctuation">;</span>  

		NdefRecord record <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">NdefRecord</span><span class="token punctuation">(</span>NdefRecord<span class="token punctuation">.</span>TNF_WELL_KNOWN<span class="token punctuation">,</span>  
		NdefRecord<span class="token punctuation">.</span>RTD_TEXT<span class="token punctuation">,</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">,</span> payload<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		NdefRecord<span class="token punctuation">[</span><span class="token punctuation">]</span> records <span class="token operator">=</span> <span class="token punctuation">{</span> record <span class="token punctuation">}</span><span class="token punctuation">;</span>  
		NdefMessage message <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">NdefMessage</span><span class="token punctuation">(</span>records<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> message<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  
</code></pre>
</div>
</body>

</html>
