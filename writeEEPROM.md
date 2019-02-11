<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>writeEEPROM</title>
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
  <div class="stackedit__html"><h2 id="how-to-write-eeprom">How to Write EEPROM</h2>
<h3 id="in-ntag_i2c_command">in Ntag_I2C_Command</h3>
<ul>
<li>
<p><strong>There are two different type of writeEEPROM</strong></p>
</li>
<li>
<p>writeEEPROM by providing byteData and writeListener, writeListener could be null<br>
- writeEEPROM by providing byteData and startAddress</p>
</li>
<li>
<p><strong>Before writing EEPROM</strong><br>
- ensure data length is lower than memory size<br>
- sector select by <strong>reader.SectorSelect</strong><br>
- get block number by <strong>Register.user_memory_Begin</strong></p>
</li>
<li>
<p><strong>After writting into EEPROM</strong><br>
- check if there’s onWriteListener for implementing some responses like progression bar to user.</p>
</li>
</ul>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>MinimalNtag_I2C_Command | Ntag_I2C_Command
<ul>
<li>writeNDEF</li>
</ul>
</li>
<li>Ntag_I2C_Command
<ul>
<li>writeDeliveryNdef</li>
</ul>
</li>
<li>Ntag_I2C_Demo
<ul>
<li>resetTagContent</li>
</ul>
</li>
</ul>
<h4 id="writeeeprom-byte-data-writeeepromlistener-listener">writeEEPROM (byte[] data, WriteEEPROMListener listener)</h4>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Override</span>  
<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeEEPROM</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data<span class="token punctuation">,</span> WriteEEPROMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException <span class="token punctuation">{</span>  
	<span class="token keyword">if</span> <span class="token punctuation">(</span>data<span class="token punctuation">.</span>length <span class="token operator">&gt;</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getMemsize</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">IOException</span><span class="token punctuation">(</span><span class="token string">"Data is to long"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  
	reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp<span class="token punctuation">;</span>  
	<span class="token keyword">int</span> index <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
	<span class="token keyword">byte</span> blockNr <span class="token operator">=</span> Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

	<span class="token comment">// write till all Data is written or the Block 0xFF was written(BlockNr  </span>
	<span class="token comment">// should be 0 then, because of the type byte)  </span>
	<span class="token keyword">for</span> <span class="token punctuation">(</span>index <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length <span class="token operator">&amp;&amp;</span> blockNr <span class="token operator">!=</span> <span class="token number">0</span><span class="token punctuation">;</span> index <span class="token operator">+=</span> <span class="token number">4</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// NTAG I2C Plus sits the Config registers in Sector 0  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus <span class="token operator">&amp;&amp;</span> blockNr <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xE2</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">break</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		temp <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> index<span class="token punctuation">,</span> index <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>temp<span class="token punctuation">,</span> blockNr<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		blockNr<span class="token operator">++</span><span class="token punctuation">;</span>  

		<span class="token comment">// Inform the listener about the writing  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>listener <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			listener<span class="token punctuation">.</span><span class="token function">onWriteEEPROM</span><span class="token punctuation">(</span>index <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>  

	<span class="token comment">// If Data is left write to the 1. Sector  </span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span>index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		blockNr <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token punctuation">;</span> index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length<span class="token punctuation">;</span> index <span class="token operator">+=</span> <span class="token number">4</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			temp <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> index<span class="token punctuation">,</span> index <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>temp<span class="token punctuation">,</span> blockNr<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			blockNr<span class="token operator">++</span><span class="token punctuation">;</span>  

			<span class="token comment">// Inform the listener about the writing  </span>
			<span class="token keyword">if</span><span class="token punctuation">(</span>listener <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				listener<span class="token punctuation">.</span><span class="token function">onWriteEEPROM</span><span class="token punctuation">(</span>index <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>  
<span class="token punctuation">}</span>
</code></pre>
<h4 id="writeeepromint-startaddr-byte-data">writeEEPROM(int startAddr, byte[] data)</h4>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeEEPROM</span><span class="token punctuation">(</span><span class="token keyword">int</span> startAddr<span class="token punctuation">,</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>  
	FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>startAddr <span class="token operator">&amp;</span> <span class="token number">0x100</span><span class="token punctuation">)</span> <span class="token operator">!=</span> <span class="token number">0x000</span> <span class="token operator">&amp;&amp;</span> <span class="token punctuation">(</span>startAddr <span class="token operator">&amp;</span> <span class="token number">0x200</span><span class="token punctuation">)</span> <span class="token operator">!=</span> <span class="token number">0x100</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">FormatException</span><span class="token punctuation">(</span><span class="token string">"Sector not supported"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>startAddr <span class="token operator">&amp;</span> <span class="token number">0x200</span><span class="token punctuation">)</span> <span class="token operator">&gt;&gt;</span> <span class="token number">16</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> index <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span> blockNr <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token punctuation">(</span>startAddr <span class="token operator">&amp;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// write till all Data is written or the Block 0xFF was written(BlockNr  </span>
		<span class="token comment">// should be 0 then, because of the type byte)  </span>
		<span class="token keyword">for</span> <span class="token punctuation">(</span>index <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length <span class="token operator">&amp;&amp;</span> blockNr <span class="token operator">!=</span> <span class="token number">0</span><span class="token punctuation">;</span> index <span class="token operator">+=</span> <span class="token number">4</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			temp <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> index<span class="token punctuation">,</span> index <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>temp<span class="token punctuation">,</span> blockNr<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			blockNr<span class="token operator">++</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token comment">// If Data is left write and the first Sector was not already written  </span>
		<span class="token comment">// switch to the first  </span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length <span class="token operator">&amp;&amp;</span> <span class="token punctuation">(</span>startAddr <span class="token operator">&amp;</span> <span class="token number">0x100</span><span class="token punctuation">)</span> <span class="token operator">!=</span> <span class="token number">0x100</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			blockNr <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
			<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token punctuation">;</span> index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length<span class="token punctuation">;</span> index <span class="token operator">+=</span> <span class="token number">4</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				temp <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> index<span class="token punctuation">,</span> index <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>temp<span class="token punctuation">,</span> blockNr<span class="token punctuation">)</span><span class="token punctuation">;</span>  
				blockNr<span class="token operator">++</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>startAddr <span class="token operator">&amp;</span> <span class="token number">0x100</span><span class="token punctuation">)</span> <span class="token operator">==</span> <span class="token number">0x100</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">IOException</span><span class="token punctuation">(</span><span class="token string">"Data is to long"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="in-minimalntag_i2c_command">in MinimalNtag_I2C_Command</h3>
<p>MinimalNtag_I2C_Command only has one implementation of writeEEPROM</p>
<h4 id="writeeeprom-byte-data-writeeepromlistener-listener-1">writeEEPROM (byte[] data, WriteEEPROMListener listener)</h4>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeEEPROM</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data<span class="token punctuation">,</span> WriteEEPROMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>  
	CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k <span class="token operator">||</span> tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span>  
		<span class="token operator">&amp;&amp;</span> data<span class="token punctuation">.</span>length <span class="token operator">&gt;</span> firstSectorMemsize<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"writeEEPROM is not Supported for this Phone, with Data bigger then First Sector("</span><span class="token operator">+</span> firstSectorMemsize <span class="token operator">+</span> <span class="token string">" Bytes)"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">if</span> <span class="token punctuation">(</span>data<span class="token punctuation">.</span>length <span class="token operator">&gt;</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getMemsize</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">IOException</span><span class="token punctuation">(</span><span class="token string">"Data is too long"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> blockNr <span class="token operator">=</span> Register<span class="token punctuation">.</span>User_memory_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// write till all Data is written  </span>
		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length<span class="token punctuation">;</span> i <span class="token operator">+=</span> <span class="token number">4</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			temp <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> i<span class="token punctuation">,</span> i <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span>blockNr<span class="token punctuation">,</span> temp<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			blockNr<span class="token operator">++</span><span class="token punctuation">;</span>  

			<span class="token comment">// Inform the listener about the writing  </span>
			<span class="token keyword">if</span><span class="token punctuation">(</span>listener <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				listener<span class="token punctuation">.</span><span class="token function">onWriteEEPROM</span><span class="token punctuation">(</span>i <span class="token operator">+</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="example-on-writeeeprom-listener">example on writeEEPROM listener</h3>
<p>writeEEPROM listener had been referenced in following files</p>
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
<h3 id="difference-between-ntag-and-minimal-ntag">Difference Between Ntag and Minimal Ntag</h3>
</div>
</body>

</html>
