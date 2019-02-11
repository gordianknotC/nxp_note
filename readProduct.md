<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>readProduct</title>
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
  <div class="stackedit__html"><h2 id="reading-product-information">Reading Product Information</h2>
<h3 id="ntag_i2c">Ntag_I2C</h3>
<h4 id="getproduct">getProduct</h4>
<p>Gets the Product of the current Tag.</p>
<h4 id="describe-implementation-in-brief">describe implementation in brief</h4>
<ul>
<li>params(none)
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>Ntag_I2C_Command | MinimalNtag_I2C_Command
<ul>
<li>writeEEPROM</li>
<li>authenticatePlus</li>
<li>protectPlus</li>
<li>unprotectPlus</li>
</ul>
</li>
<li>Ntag_I2C_Command
<ul>
<li>constructor</li>
<li>getSessionRegisters</li>
<li>getConfigRegisters</li>
<li>writeConfigRegisters</li>
<li>writeEEPROM</li>
<li>readEEPROM</li>
<li>writeSRAMBlock</li>
<li>writeDeliveryNdef</li>
</ul>
</li>
<li>Ntag_I2C_Demo
<ul>
<li>constructor</li>
<li>resetTagMemory</li>
<li>getRegister_Settings</li>
<li>readWriteConfigRegister</li>
<li>writeConfigRegisters</li>
<li>readTagContent</li>
<li>resetTagContent</li>
<li>ObtainAuthStatus</li>
<li>SRAMSpeedtestTask / doInBackground</li>
<li>EEPROMSpeedtestTask / onPreExecute</li>
<li>EEPROMSpeedtestTask / doInBackground</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> Prod <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>getVersionResponse <span class="token operator">==</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">try</span> <span class="token punctuation">{</span>  
				getVersionResponse <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_Get_Version</span><span class="token punctuation">(</span>reader<span class="token punctuation">.</span><span class="token function">getVersion</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token keyword">try</span> <span class="token punctuation">{</span>  
					reader<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					reader<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

					<span class="token keyword">if</span> <span class="token punctuation">(</span>temp<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x04</span> <span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">12</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xE1</span>  
					<span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">13</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x10</span>  
					<span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">14</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x6D</span>  
					<span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">15</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						temp <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xE8</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
						getVersionResponse <span class="token operator">=</span> Ntag_Get_Version<span class="token punctuation">.</span>NTAG_I2C_1k<span class="token punctuation">;</span>  

					<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>temp<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x04</span>  
						<span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">12</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xE1</span>  
						<span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">13</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x10</span>  
						<span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">14</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xEA</span>  
						<span class="token operator">&amp;&amp;</span> temp<span class="token punctuation">[</span><span class="token number">15</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						getVersionResponse <span class="token operator">=</span> Ntag_Get_Version<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">;</span>  
					<span class="token punctuation">}</span>  
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">FormatException</span> e2<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					reader<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					reader<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					e2<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					getVersionResponse <span class="token operator">=</span> Ntag_Get_Version<span class="token punctuation">.</span>NTAG_I2C_1k<span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">return</span> getVersionResponse<span class="token punctuation">.</span><span class="token function">Get_Product</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h3 id="minimalntag_i2c">MinimalNtag_I2C</h3>
<p>product information in MinimalNtag was initialized by its constructor</p>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> Prod <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException <span class="token punctuation">{</span>  
	  <span class="token comment">// returns generic NTAG_I2C_1k, because getVersion is not possible  </span>
	  <span class="token keyword">return</span> tagType<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
</div>
</body>

</html>
