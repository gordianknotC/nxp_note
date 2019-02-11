<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>writeSRAM</title>
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
  <div class="stackedit__html"><h1 id="how-write-sram-implements">How Write SRAM Implements</h1>
<h2 id="in-ntag_i2c_command">in Ntag_I2C_Command</h2>
<h3 id="writesram">writeSRAM</h3>
<h4 id="describe-writing-process-in-brief">describe writing process in brief</h4>
<ul>
<li>
<p><strong>params</strong></p>
<ul>
<li><strong>byte[]</strong> data</li>
<li><strong>WriteSRAMListener</strong> listenr
<ul>
<li>A listener for implementing user responses, e.g. showing writing process to user</li>
</ul>
</li>
</ul>
</li>
<li>
<p>Calculate data blocks by dividing data into dataBlocks of each 64 bytes</p>
</li>
<li>
<p>Write each dataBlock by <strong>writeSRAMBlock</strong></p>
</li>
<li>
<p>Check if Ntag is in <strong>Polling_Mode</strong> to determine whether needs to wait for device or not</p>
<h5 id="about-polling-mode">About Polling Mode</h5>
<p>Polling is the process where the computer or controlling device waits for an <a href="https://en.wikipedia.org/wiki/External_device" title="External device">external device</a> to check for its readiness or state, often with low-level hardware.</p>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>Ntag_Demo / Flash</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeSRAM</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data<span class="token punctuation">,</span> R_W_Methods method<span class="token punctuation">,</span> WriteSRAMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> TimeoutException <span class="token punctuation">{</span>  
		<span class="token keyword">int</span> blocks <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">int</span><span class="token punctuation">)</span> Math<span class="token punctuation">.</span><span class="token function">ceil</span><span class="token punctuation">(</span>data<span class="token punctuation">.</span>length <span class="token operator">/</span> <span class="token number">64.0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> blocks<span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> dataBlock <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">64</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span>data<span class="token punctuation">.</span>length <span class="token operator">-</span> <span class="token punctuation">(</span>i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">)</span> <span class="token operator">*</span> <span class="token number">64</span> <span class="token operator">&lt;</span> <span class="token number">0</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				Arrays<span class="token punctuation">.</span><span class="token function">fill</span><span class="token punctuation">(</span>dataBlock<span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				System<span class="token punctuation">.</span><span class="token function">arraycopy</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> i <span class="token operator">*</span> <span class="token number">64</span><span class="token punctuation">,</span> dataBlock<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> data<span class="token punctuation">.</span>length <span class="token operator">%</span> <span class="token number">64</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				System<span class="token punctuation">.</span><span class="token function">arraycopy</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> i <span class="token operator">*</span> <span class="token number">64</span><span class="token punctuation">,</span> dataBlock<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">64</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
			<span class="token function">writeSRAMBlock</span><span class="token punctuation">(</span>dataBlock<span class="token punctuation">,</span> listener<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span>method <span class="token operator">==</span> R_W_Methods<span class="token punctuation">.</span>Polling_Mode<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token function">waitforI2Cread</span><span class="token punctuation">(</span><span class="token number">100</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				<span class="token keyword">try</span> <span class="token punctuation">{</span>  
				<span class="token comment">// else wait  </span>
				Thread<span class="token punctuation">.</span><span class="token function">sleep</span><span class="token punctuation">(</span><span class="token number">6</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>   
</code></pre>
<h3 id="writesramblock">writeSRAMBlock</h3>
<h4 id="describe-writing-process-in-brief-1">describe writing process in brief</h4>
<ul>
<li>
<p><strong>params</strong></p>
<ul>
<li><strong>byte[]</strong> data</li>
<li><strong>WriteSRAMListener</strong> listener
<ul>
<li>A listener for implementing user responses, e.g. inform  to user about the writing process</li>
</ul>
</li>
</ul>
</li>
<li>
<p>get <strong>sramSector</strong></p>
<ul>
<li>about sramSector<br>
sramSector could be 1 or zero depends on different type of nxp product (1) Ntag_I2C_2k (0) …</li>
</ul>
</li>
<li>
<p>determine if it can be applied on fast_write mode</p>
<ul>
<li>only NTAG_I2C_X_Plus has fast_write/ fast_read mode</li>
<li>in fast_write, data is directly written in SRAM</li>
<li>only non-authenticated state in AuthState can be applied into fast_write</li>
</ul>
</li>
<li>
<p>if not, use MifareUltralight</p>
<h5 id="referenced-in-1">referenced in</h5>
<pre><code>  - Ntag_I2C_Demo / setBoardVersion
  - Ntag_I2C_Demo / doInBackground
  - Ntag_I2C_Command / writeSRAM
  - MinimalNtag_I2C_Command / writeSRAM
  - 
</code></pre>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeSRAMBlock</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data<span class="token punctuation">,</span> WriteSRAMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> txBuffer <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">int</span> index <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span>sramSector<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token comment">/*
		* Samsung controllers do not like NfcA Transceive method, 
		* so it is better using MUL writePage command when possible 
		* For NTAG_I2C_2k it is not possible to use MUL commands because when 
		* establishing the connection the Sector is moved back to 0 */</span>  
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus 
			<span class="token operator">||</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span>  
			<span class="token operator">&amp;&amp;</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			reader<span class="token punctuation">.</span><span class="token function">fast_write</span><span class="token punctuation">(</span>
				data<span class="token punctuation">,</span> 
				<span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> Register<span class="token punctuation">.</span>SRAM_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>  
				<span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token punctuation">(</span>Register<span class="token punctuation">.</span>SRAM_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token number">0x0F</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				reader<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				MifareUltralight ul <span class="token operator">=</span> MifareUltralight<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span>tag<span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token keyword">if</span> <span class="token punctuation">(</span>ul <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					ul<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					<span class="token keyword">int</span> SRAM_Begin <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">int</span><span class="token punctuation">)</span> Register<span class="token punctuation">.</span>SRAM_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">&amp;</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
					<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">16</span><span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> dI <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> dI <span class="token operator">&lt;</span> <span class="token number">4</span><span class="token punctuation">;</span> dI<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
							<span class="token keyword">if</span> <span class="token punctuation">(</span>index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
								txBuffer<span class="token punctuation">[</span>dI<span class="token punctuation">]</span> <span class="token operator">=</span> data<span class="token punctuation">[</span>index<span class="token operator">++</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
							<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
								txBuffer<span class="token punctuation">[</span>dI<span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
							<span class="token punctuation">}</span>  
						<span class="token punctuation">}</span>  
						ul<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span>SRAM_Begin <span class="token operator">+</span> i<span class="token punctuation">,</span> txBuffer<span class="token punctuation">)</span><span class="token punctuation">;</span>  
					<span class="token punctuation">}</span>  
					ul<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
				reader<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					<span class="token keyword">try</span> <span class="token punctuation">{</span>  
						<span class="token comment">// NTAG_I2C_1k_Plus | NTAG_I2C_2k_Plus</span>
						<span class="token function">authenticatePlus</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
					<span class="token punctuation">}</span>  
				<span class="token punctuation">}</span>  
				<span class="token comment">// if not authenticated and not one of (NTAG_I2C_1k_Plus | NTAG_I2C_2k_Plus)</span>
				<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">16</span><span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> dI <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> dI <span class="token operator">&lt;</span> <span class="token number">4</span><span class="token punctuation">;</span> dI<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						<span class="token keyword">if</span> <span class="token punctuation">(</span>index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
							txBuffer<span class="token punctuation">[</span>dI<span class="token punctuation">]</span> <span class="token operator">=</span> data<span class="token punctuation">[</span>index<span class="token operator">++</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
						<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
							txBuffer<span class="token punctuation">[</span>dI<span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
						<span class="token punctuation">}</span>  
					<span class="token punctuation">}</span>  
					reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>txBuffer<span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token punctuation">(</span>Register<span class="token punctuation">.</span>SRAM_Begin<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> i<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
		<span class="token comment">// Inform the listener about the writing  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>listener <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			listener<span class="token punctuation">.</span><span class="token function">onWriteSRAM</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<hr>
<h2 id="in-minimalntag_i2c_command">in MinimalNtag_I2C_Command</h2>
<h3 id="writesram-1">writeSRAM</h3>
<h4 id="describe-writing-process-in-brief-2">describe writing process in brief</h4>
<ul>
<li>
<p><strong>params</strong></p>
<ul>
<li><strong>byte[]</strong> data</li>
<li><strong>WriteSRAMListener</strong> listenr
<ul>
<li>A listener for implementing user responses, e.g. showing writing process to user</li>
</ul>
</li>
</ul>
</li>
<li>
<p>MinimalNtag not support for NTAG_I2C_2k</p>
</li>
<li>
<p>Calculate data blocks by dividing data into dataBlocks of each 64 bytes</p>
</li>
<li>
<p>Write each dataBlock by <strong>writeSRAMBlock</strong></p>
</li>
<li>
<p>Check if Ntag is in <strong>Polling_Mode</strong> to determine whether needs to wait for device or not</p>
<h5 id="referenced-in-2">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo / setBoardVersion</li>
<li>Ntag_I2C_Demo / doInBackground</li>
<li>Ntag_I2C_Command / writeSRAM</li>
<li>MinimalNtag_I2C_Command / writeSRAM</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeSRAM</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data<span class="token punctuation">,</span> R_W_Methods method<span class="token punctuation">,</span> WriteSRAMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"writeSRAM is not Supported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">int</span> blocks <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">int</span><span class="token punctuation">)</span> Math<span class="token punctuation">.</span><span class="token function">ceil</span><span class="token punctuation">(</span>data<span class="token punctuation">.</span>length <span class="token operator">/</span> <span class="token number">64.0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> blocks<span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token function">writeSRAMBlock</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> listener<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span>method <span class="token operator">==</span> R_W_Methods<span class="token punctuation">.</span>Polling_Mode<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token function">waitforI2Cread</span><span class="token punctuation">(</span><span class="token number">100</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				<span class="token keyword">try</span> <span class="token punctuation">{</span>  
					<span class="token comment">// else wait  </span>
					Thread<span class="token punctuation">.</span><span class="token function">sleep</span><span class="token punctuation">(</span><span class="token number">6</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">Exception</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span>  
			<span class="token keyword">if</span> <span class="token punctuation">(</span>data<span class="token punctuation">.</span>length <span class="token operator">&gt;</span> <span class="token number">64</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				data <span class="token operator">=</span> Arrays<span class="token punctuation">.</span><span class="token function">copyOfRange</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> <span class="token number">64</span><span class="token punctuation">,</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h3 id="writesramblock-1">writeSRAMBlock</h3>
<h4 id="describe-writing-process-in-brief-3">describe writing process in brief</h4>
<ul>
<li>
<p><strong>params</strong></p>
<ul>
<li><strong>byte[]</strong> data</li>
<li><strong>WriteSRAMListener</strong> listener
<ul>
<li>A listener for implementing user responses, e.g. inform  to user about the writing process</li>
</ul>
</li>
</ul>
</li>
<li>
<p>MinimalNtag not support for NTAG_I2C_2k</p>
</li>
<li>
<p>mfu.<strong>writePage</strong></p>
<h5 id="referenced-in-3">referenced in</h5>
<ul>
<li>Ntag_Demo / Flash</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">writeSRAMBlock</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data<span class="token punctuation">,</span> WriteSRAMListener listener<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>  
		FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"writeSRAMBlock is not Supported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> txBuffer <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
		<span class="token keyword">int</span> index <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  

		<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token number">16</span><span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">int</span> dI <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> dI <span class="token operator">&lt;</span> <span class="token number">4</span><span class="token punctuation">;</span> dI<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token keyword">if</span> <span class="token punctuation">(</span>index <span class="token operator">&lt;</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
					txBuffer<span class="token punctuation">[</span>dI<span class="token punctuation">]</span> <span class="token operator">=</span> data<span class="token punctuation">[</span>index<span class="token operator">++</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
					txBuffer<span class="token punctuation">[</span>dI<span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
				<span class="token punctuation">}</span>  
			<span class="token punctuation">}</span>  
			mfu<span class="token punctuation">.</span><span class="token function">writePage</span><span class="token punctuation">(</span><span class="token number">0xF0</span> <span class="token operator">+</span> i<span class="token punctuation">,</span> txBuffer<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token comment">// Inform the listener about the writing  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>listener <span class="token operator">!=</span> null<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			listener<span class="token punctuation">.</span><span class="token function">onWriteSRAM</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="writesram-listener">writeSRAM listener</h3>
<p>WriteSRAMListener has been referenced in following files</p>
<ul>
<li>Ntag_Demo / onWriteSRAM</li>
<li>Ntag_I2C_Command / writeSRAMBlock</li>
<li>MinimalNtag_I2C_Command / writeSRAMBlock</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onWriteSRAM</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		main<span class="token punctuation">.</span><span class="token function">runOnUiThread</span><span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Runnable</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token annotation punctuation">@Override</span>  
			<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">run</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				FlashMemoryActivity<span class="token punctuation">.</span><span class="token function">updateFLashDialog</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<h3 id="r_w_methods">R_W_Methods</h3>
<p>there are three R/W modes in Ntag</p>
<ul>
<li>fast_mode</li>
<li>polling_mode</li>
<li>error mode</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">public</span> <span class="token keyword">enum</span> R_W_Methods <span class="token punctuation">{</span>  
	   Fast_Mode<span class="token punctuation">,</span> Polling_Mode<span class="token punctuation">,</span> Error  
	<span class="token punctuation">}</span> ##### ReadMemoryActivity 
</code></pre>
</div>
</body>

</html>
