<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>readEEPROM</title>
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
  <div class="stackedit__html"><h1 id="how-readeeprom-implements">How ReadEEPROM implements</h1>
<h2 id="in-ntag_i2c_command">in Ntag_I2C_Command</h2>
<h3 id="readeeprom">readEEPROM</h3>
<p>Read Data from the EEPROM.</p>
<ul>
<li>
<p>params</p>
<ul>
<li><strong>int</strong> absStart, Start of the read</li>
<li><strong>int</strong> absEnd, End of the read(included in the Answer)</li>
</ul>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>MinimalNtag_I2C_Command | Ntag_I2C_Command
<ul>
<li>readNDEF</li>
</ul>
</li>
<li>Ntag_I2C_Demo
<ul>
<li>doInBackground</li>
<li>readTagContent</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">readEEPROM</span><span class="token punctuation">(</span><span class="token keyword">int</span> absStart<span class="token punctuation">,</span> <span class="token keyword">int</span> absEnd<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">int</span> maxfetchsize <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">getMaxTransceiveLength</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">int</span> maxFastRead <span class="token operator">=</span> <span class="token punctuation">(</span>maxfetchsize <span class="token operator">-</span> <span class="token number">2</span><span class="token punctuation">)</span> <span class="token operator">/</span> <span class="token number">4</span><span class="token punctuation">;</span>  
		<span class="token keyword">int</span> fetchStart <span class="token operator">=</span> absStart<span class="token punctuation">;</span>  
		<span class="token keyword">int</span> fetchEnd <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data <span class="token operator">=</span> null<span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp <span class="token operator">=</span> null<span class="token punctuation">;</span>  

		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token keyword">while</span> <span class="token punctuation">(</span>fetchStart <span class="token operator">&lt;=</span> absEnd<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span> see detail 
		<span class="token punctuation">}</span>  
		<span class="token comment">// Let's go back to Sector 0  </span>
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> data<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>	
</code></pre>
<h2 id="in-minimalntag_i2c_command">in MinimalNtag_I2C_Command</h2>
<h3 id="readeeprom-1">readEEPROM</h3>
<p>Read Data from the EEPROM.</p>
<ul>
<li>
<p>params</p>
<ul>
<li><strong>int</strong> absStart, Start of the read</li>
<li><strong>int</strong> absEnd, End of the read(included in the Answer)</li>
</ul>
<h5 id="referenced-in-1">referenced in</h5>
<ul>
<li>MinimalNtag_I2C_Command | Ntag_I2C_Command
<ul>
<li>readNDEF</li>
</ul>
</li>
<li>Ntag_I2C_Demo
<ul>
<li>doInBackground</li>
<li>readTagContent</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">readEEPROM</span><span class="token punctuation">(</span><span class="token keyword">int</span> absStart<span class="token punctuation">,</span> <span class="token keyword">int</span> absEnd<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>  
		FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k <span class="token operator">&amp;&amp;</span> absEnd <span class="token operator">&gt;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span>  
			<span class="token operator">||</span> tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus <span class="token operator">&amp;&amp;</span> absEnd <span class="token operator">&gt;</span> <span class="token number">0xE1</span><span class="token punctuation">)</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"readEEPROM is not Supported for this Phone on Second Sector"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		answer <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  

		<span class="token keyword">if</span> <span class="token punctuation">(</span>absStart <span class="token operator">&gt;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			absStart <span class="token operator">=</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">if</span> <span class="token punctuation">(</span>absEnd <span class="token operator">&gt;</span> <span class="token number">0xFF</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			absEnd <span class="token operator">=</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">int</span> i<span class="token punctuation">;</span>  
		<span class="token keyword">for</span> <span class="token punctuation">(</span>i <span class="token operator">=</span> absStart<span class="token punctuation">;</span> i <span class="token operator">&lt;=</span> <span class="token punctuation">(</span>absEnd <span class="token operator">-</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">;</span> i <span class="token operator">+=</span> <span class="token number">4</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			temp <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span>i<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			answer <span class="token operator">=</span> <span class="token function">concat</span><span class="token punctuation">(</span>answer<span class="token punctuation">,</span> temp<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">if</span> <span class="token punctuation">(</span>i <span class="token operator">&lt;</span> absEnd<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			temp <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span>absEnd <span class="token operator">-</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> bla <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>temp<span class="token punctuation">,</span> <span class="token punctuation">(</span>i <span class="token operator">-</span> <span class="token punctuation">(</span>absEnd <span class="token operator">-</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token operator">*</span> <span class="token number">4</span><span class="token punctuation">,</span> <span class="token number">16</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			answer <span class="token operator">=</span> <span class="token function">concat</span><span class="token punctuation">(</span>answer<span class="token punctuation">,</span> bla<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">return</span> answer<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong>Detail in Ntag_I2C_Command</strong></li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	fetchEnd <span class="token operator">=</span> fetchStart <span class="token operator">+</span> maxFastRead <span class="token operator">-</span> <span class="token number">1</span><span class="token punctuation">;</span>  
	<span class="token comment">// check for last read, fetch only rest  </span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span>fetchEnd <span class="token operator">&gt;</span> absEnd<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		fetchEnd <span class="token operator">=</span> absEnd<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	<span class="token comment">// check for sector change in between and reduce fast_read to stay within sector  </span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>fetchStart <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span> <span class="token operator">!=</span> <span class="token punctuation">(</span>fetchEnd <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			fetchEnd <span class="token operator">=</span> <span class="token punctuation">(</span>fetchStart <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>fetchStart <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span> <span class="token operator">==</span> <span class="token number">0</span> <span class="token operator">&amp;&amp;</span> <span class="token punctuation">(</span>fetchEnd <span class="token operator">&gt;</span> <span class="token number">0xE2</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			fetchEnd <span class="token operator">=</span> <span class="token punctuation">(</span>fetchStart <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token number">0xE1</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>  
	temp <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">fast_read</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token punctuation">(</span>fetchStart <span class="token operator">&amp;</span> <span class="token number">0x00FF</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token punctuation">(</span>fetchEnd <span class="token operator">&amp;</span> <span class="token number">0x00FF</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	data <span class="token operator">=</span> <span class="token function">concat</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> temp<span class="token punctuation">)</span><span class="token punctuation">;</span>  

	<span class="token comment">// calculate next fetch_start  </span>
	fetchStart <span class="token operator">=</span> fetchEnd <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span>  

	<span class="token comment">// check for sector change in between and reduce fast_read to stay within sector  </span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>fetchStart <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span> <span class="token operator">!=</span> <span class="token punctuation">(</span>fetchEnd <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>fetchStart <span class="token operator">&amp;</span> <span class="token number">0xFF00</span><span class="token punctuation">)</span> <span class="token operator">==</span> <span class="token number">0</span> <span class="token operator">&amp;&amp;</span> <span class="token punctuation">(</span>fetchEnd <span class="token operator">&gt;=</span> <span class="token number">0xE1</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			fetchStart <span class="token operator">=</span> <span class="token number">0x100</span><span class="token punctuation">;</span>  

			<span class="token comment">// Update the absEnd with pages not read on Sector 0  </span>
			absEnd <span class="token operator">=</span> absEnd <span class="token operator">+</span> <span class="token punctuation">(</span><span class="token number">0xFF</span> <span class="token operator">-</span> <span class="token number">0xE2</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span> 
</code></pre>
</div>
</body>

</html>
