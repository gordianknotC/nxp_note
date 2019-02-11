<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>writeRegisters</title>
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
  <div class="stackedit__html"><h1 id="how-write-auth--config-registers-imlements">How Write Auth | Config Registers Imlements</h1>
<h2 id="write-auth-registers">Write Auth Registers</h2>
<h3 id="ntag_i2c_command">Ntag_I2C_Command</h3>
<h4 id="writeauthregisters">writeAuthRegisters</h4>
<h4 id="describe-writing-process-in-brief">describe writing process in brief</h4>
<ul>
<li>params
<ul>
<li><strong>byte</strong> auth0</li>
<li><strong>byte</strong> access</li>
<li><strong>byte</strong> ptI2C</li>
</ul>
</li>
<li>write following configurations by reader.<strong>write</strong>
<ul>
<li>write access configuration</li>
<li>write PT(Pass Thru) configuration</li>
<li>write password</li>
<li>write auth0</li>
</ul>
</li>
</ul>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>resetTagMemory</li>
<li>writeConfigRegisters</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeAuthRegisters</span><span class="token punctuation">(</span><span class="token keyword">byte</span> auth0<span class="token punctuation">,</span> <span class="token keyword">byte</span> access<span class="token punctuation">,</span> <span class="token keyword">byte</span> ptI2C<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
	  <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	  
	  reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Write the ACCESS configuration  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> access<span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>ACCESS<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Write the PT I2C configuration  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> ptI2C<span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>PT_I2C<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Set the password to FFs  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>PWD<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Set the pack to 00s  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>PACK<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> auth0<span class="token punctuation">;</span>  
	  reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>AUTH0<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h3 id="minimalntag_i2c_command">MinimalNtag_I2C_Command</h3>
<h4 id="writeauthregisters-1">writeAuthRegisters</h4>
<h4 id="describe-writing-process-in-brief-1">describe writing process in brief</h4>
<ul>
<li>params
<ul>
<li><strong>byte</strong> auth0</li>
<li><strong>byte</strong> access</li>
<li><strong>byte</strong> ptI2C</li>
</ul>
</li>
<li>write following configurations by mfu.<strong>writePage</strong>
<ul>
<li>write access configuration</li>
<li>write PT(Pass Thru) configuration</li>
<li>write password</li>
<li>write auth0</li>
</ul>
</li>
</ul>
<h5 id="referenced-in-1">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>resetTagMemory</li>
<li>writeConfigRegisters</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeAuthRegisters</span><span class="token punctuation">(</span><span class="token keyword">byte</span> auth0<span class="token punctuation">,</span> <span class="token keyword">byte</span> access<span class="token punctuation">,</span> <span class="token keyword">byte</span> ptI2C<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
	  <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Write the ACCESS configuration  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> access<span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xE4</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Write the PT I2C configuration  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> ptI2C<span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xE7</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Set the password to FFs  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	  mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xE5</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  <span class="token comment">// Set the pack to 00s  </span>
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xE6</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	  
	  data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	  data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> auth0<span class="token punctuation">;</span>  
	  mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xE3</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h2 id="write-config-registers">Write Config Registers</h2>
<h3 id="ntag_i2c_command-1">Ntag_I2C_Command</h3>
<h4 id="writeconfigregisters">writeConfigRegisters</h4>
<h5 id="referenced-in-2">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>resetTagMemory</li>
<li>writeConfigRegisters</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeConfigRegisters</span><span class="token punctuation">(</span><span class="token keyword">byte</span> NC_R<span class="token punctuation">,</span> <span class="token keyword">byte</span> LD_R<span class="token punctuation">,</span> <span class="token keyword">byte</span> SM_R<span class="token punctuation">,</span> <span class="token keyword">byte</span> WD_LS_R<span class="token punctuation">,</span> <span class="token keyword">byte</span> WD_MS_R<span class="token punctuation">,</span> <span class="token keyword">byte</span> I2C_CLOCK_STR<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> Data <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k 
		<span class="token operator">||</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus 
		<span class="token operator">||</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">else</span> <span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">IOException</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		
		<span class="token comment">// Write the Config Regs  </span>
		Data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> NC_R<span class="token punctuation">;</span>  
		Data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> LD_R<span class="token punctuation">;</span>  
		Data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> SM_R<span class="token punctuation">;</span>  
		Data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> WD_LS_R<span class="token punctuation">;</span>  
		reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>Data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>Configuration<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		Data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> WD_MS_R<span class="token punctuation">;</span>  
		Data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> I2C_CLOCK_STR<span class="token punctuation">;</span>  
		Data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		Data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>Data<span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token punctuation">(</span>Register<span class="token punctuation">.</span>Configuration<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h3 id="minimalntag_i2c_command-1">MinimalNtag_I2C_Command</h3>
<h4 id="writeconfigregisters-1">writeConfigRegisters</h4>
<h5 id="referenced-in-3">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>resetTagMemory</li>
<li>writeConfigRegisters</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeConfigRegisters</span><span class="token punctuation">(</span><span class="token keyword">byte</span> ncR<span class="token punctuation">,</span> <span class="token keyword">byte</span> ldR<span class="token punctuation">,</span> <span class="token keyword">byte</span> smR<span class="token punctuation">,</span> <span class="token keyword">byte</span> wdLsR<span class="token punctuation">,</span> <span class="token keyword">byte</span> wdMsR<span class="token punctuation">,</span> <span class="token keyword">byte</span> i2CClockStr<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"writeConfigRegisters is not Supported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  

		<span class="token comment">// Write the Config Regs  </span>
		data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> ncR<span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> ldR<span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> smR<span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> wdLsR<span class="token punctuation">;</span>  
		mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xE8</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>  

		data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> wdMsR<span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> i2CClockStr<span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xE9</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
</div>
</body>

</html>
