<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>readSRAM</title>
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
  <div class="stackedit__html"><h2 id="how-readsram-implements">How readSRAM implements</h2>
<ul>
<li>
<h3 id="in-ntag_i2c_command">in Ntag_I2C_Command</h3>
</li>
<li>
<h4 id="readsram">readSRAM</h4>
</li>
<li>readSRAM by specifying blocks(int) and methods (RW mode)</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">readSRAM</span><span class="token punctuation">(</span><span class="token keyword">int</span> blocks<span class="token punctuation">,</span> R_W_Methods method<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> TimeoutException <span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> response <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp<span class="token punctuation">;</span>  

		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> blocks<span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span>method <span class="token operator">==</span> R_W_Methods<span class="token punctuation">.</span>Polling_Mode<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token function">waitforI2Cwrite</span><span class="token punctuation">(</span><span class="token number">100</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				<span class="token keyword">try</span> <span class="token punctuation">{</span>  
					<span class="token comment">// else wait  </span>
					Thread<span class="token punctuation">.</span><span class="token function">sleep</span><span class="token punctuation">(</span><span class="token number">6</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span> 
			temp <span class="token operator">=</span> <span class="token function">readSRAMBlock</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

			<span class="token comment">// concat read block to the full response  </span>
			response <span class="token operator">=</span> <span class="token function">concat</span><span class="token punctuation">(</span>response<span class="token punctuation">,</span> temp<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		answer <span class="token operator">=</span> response<span class="token punctuation">;</span>  
		<span class="token keyword">return</span> response<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h4 id="readsramblock">readSRAMBlock</h4>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">readSRAMBlock</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException <span class="token punctuation">{</span>  
		answer <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">try</span> <span class="token punctuation">{</span>  
				<span class="token comment">// authenticate using reader.pwdAuth  </span>
				<span class="token function">authenticatePlus</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span>sramSector<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		answer <span class="token operator">=</span> reader<span class="token punctuation">.</span><span class="token function">fast_read</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xF0</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> answer<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>about sramSector<br>
sramSector could be 1 or zero depends on different type of nxp product
<ul>
<li>1: Ntag_I2C_2k</li>
<li>0: …</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  sramSector <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
  sramSector <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
<hr>
<ul>
<li>
<h3 id="in-minimalntag_i2c_command">in MinimalNtag_I2C_Command</h3>
</li>
<li>
<h4 id="readsram-1">readSRAM</h4>
</li>
<li>readSRAM by specifying blocks(int) and methods (RW mode)</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">readSRAM</span><span class="token punctuation">(</span><span class="token keyword">int</span> blocks<span class="token punctuation">,</span> R_W_Methods method<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>  
		FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"readSRAM is not Supported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> response <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> temp<span class="token punctuation">;</span>  
		answer <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> blocks<span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span>method <span class="token operator">==</span> R_W_Methods<span class="token punctuation">.</span>Polling_Mode<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token function">waitforI2Cwrite</span><span class="token punctuation">(</span><span class="token number">100</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				<span class="token keyword">try</span> <span class="token punctuation">{</span>  
					<span class="token comment">// else wait  </span>
					Thread<span class="token punctuation">.</span><span class="token function">sleep</span><span class="token punctuation">(</span><span class="token number">6</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span> 
			temp <span class="token operator">=</span> <span class="token function">readSRAMBlock</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token comment">// concat read block to the full response  </span>
			response <span class="token operator">=</span> <span class="token function">concat</span><span class="token punctuation">(</span>response<span class="token punctuation">,</span> temp<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		answer <span class="token operator">=</span> response<span class="token punctuation">;</span>  
		<span class="token keyword">return</span> response<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>	 
</code></pre>
<ul>
<li>
<h4 id="readsramblock-1">readSRAMBlock</h4>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">readSRAMBlock</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"readSRAMBlock is not Supported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		answer <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">0x0F</span><span class="token punctuation">;</span> i <span class="token operator">+=</span> <span class="token number">4</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			answer <span class="token operator">=</span> <span class="token function">concat</span><span class="token punctuation">(</span>answer<span class="token punctuation">,</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xF0</span> <span class="token operator">+</span> i<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">return</span> answer<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>	 
</code></pre>
</div>
</body>

</html>
