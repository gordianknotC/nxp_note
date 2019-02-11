<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>readRegisters</title>
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
  <div class="stackedit__html"><h2 id="reading-configregisterssessionregisters...">Reading configRegisters/sessionRegisters/…</h2>
<h3 id="in-ntag_i2c_command">in Ntag_I2C_Command</h3>
<ul>
<li>
<h4 id="session-registers---getsessionregister">Session Registers - getSessionRegister</h4>
</li>
<li>
<h4 id="describe-writing-process-in-brief">describe writing process in brief</h4>
<ul>
<li>
<p>params</p>
<ul>
<li><strong>SR_Offset</strong> off
<ul>
<li>offset  of the session registers</li>
</ul>
</li>
</ul>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li></li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token comment">// referenced from  </span>
	<span class="token comment">// 1) Ntag_I2C_Demo / .setBoardVersion()  </span>
		<span class="token operator">-</span><span class="token comment">// 2) Ntag_I2C_Command / .checkPTwritePossible()</span>

```java
	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span> <span class="token function">getSessionRegister</span><span class="token punctuation">(</span>SR_Offset off<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>FormatException<span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> register <span class="token operator">=</span> <span class="token function">getSessionRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> register<span class="token punctuation">[</span>off<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>

	<span class="token comment">// referenced from Ntag_Demo.getSessionRegisters  </span>
	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getSessionRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus 
			<span class="token operator">||</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span>Register<span class="token punctuation">.</span>Session_PLUS<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span>Register<span class="token punctuation">.</span>Session<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li></li>
</ul>
<h4 id="config-registers---getconfigregisters">Config Registers - getConfigRegisters</h4>
<ul>
<li>
<h4 id="describe-writing-process-in-brief-1">describe writing process in brief</h4>
<ul>
<li>params (none)</li>
</ul>
<h5 id="referenced-in-1">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>readWriteConfigRegister</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getConfigRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException <span class="token punctuation">{</span>  
		<span class="token comment">/*  
		NTAG I2C plus behavior can be configured and read in two separate locations depending if the configurations shall be effective within the communication session (use session registers) or by default after Power-On Reset (POR) (use configuration registers). */</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k 
		<span class="token operator">||</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus  		
		<span class="token operator">||</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">else</span> <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span>  
			reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">else</span> <span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">IOException</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span>Register<span class="token punctuation">.</span>Configuration<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>


	<span class="token comment">// this function has not been used  </span>
	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span> <span class="token function">getConfigRegister</span><span class="token punctuation">(</span>CR_Offset off<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>  
		FormatException <span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> register <span class="token operator">=</span> <span class="token function">getConfigRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> register<span class="token punctuation">[</span>off<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h4 id="pt--auth--acess-registers">PT | Auth | Acess Registers</h4>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getAuth0Register</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span>Register<span class="token punctuation">.</span>AUTH0<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getAccessRegister</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span>Register<span class="token punctuation">.</span>ACCESS<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getPTI2CRegister</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">read</span><span class="token punctuation">(</span>Register<span class="token punctuation">.</span>PT_I2C<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<hr>
<h3 id="in-minimalntag_i2c_command">in MinimalNtag_I2C_Command</h3>
<h4 id="session-register---getsessionregisters">Session Register - getSessionRegisters</h4>
<ul>
<li>
<h4 id="describe-writing-process-in-brief-2">describe writing process in brief</h4>
<ul>
<li>
<p>params</p>
<ul>
<li><strong>SR_Offset</strong> off
<ul>
<li>offset  of the session registers</li>
</ul>
</li>
</ul>
<h5 id="referenced-in-2">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo.setBoardVersion()</li>
<li>Ntag_I2C_Command.checkPTwritePossible()</li>
</ul>
</li>
</ul>
</li>
</ul>
<hr>
<h3 id="in-minimalntag_i2c_command-1">in MinimalNtag_I2C_Command</h3>
<ul>
<li>
<h4 id="config-registers">Config Registers</h4>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getSessionConfigRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>  
		CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k <span class="token operator">||</span> tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"getSessionConfigRegisters is not sSupported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xEC8</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> answer<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  


	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span> <span class="token function">getSessionConfigRegister</span><span class="token punctuation">(</span>SCR_Offset off<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>  
		FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k <span class="token operator">||</span> tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"getSessionConfigRegister is not sSupported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">return</span> <span class="token function">getSessionConfigRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">[</span>off<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h4 id="config-registers---getconfigregisters-1">Config Registers - getConfigRegisters</h4>
<ul>
<li>
<h4 id="describe-writing-process-in-brief-3">describe writing process in brief</h4>
<ul>
<li>params (none)</li>
</ul>
<h5 id="referenced-in-3">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>readWriteConfig- ### Session Register</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getConfigSessionRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>  
		CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k <span class="token operator">||</span> tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"getConfigSessionRegisters is not Ssupported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xE8C</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">return</span> answer<span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  


	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span> <span class="token function">getConfigSessionRegister</span><span class="token punctuation">(</span>CSR_Offset off<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span>  
		FormatException<span class="token punctuation">,</span> CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span>tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k <span class="token operator">||</span> tagType <span class="token operator">==</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">CommandNotSupportedException</span><span class="token punctuation">(</span><span class="token string">"getConfigSessionRegister is not Ssupported for this Phone with NTAG I2C 2k"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">return</span> <span class="token function">getConfigSessionRegisters</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">[</span>off<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h4 id="pt--auth--acess-registers-1">PT | Auth | Acess Registers</h4>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getAuth0Register</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">return</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xE3</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getAccessRegister</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">return</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xE4</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  

	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">getPTI2CRegister</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span>CommandNotSupportedException <span class="token punctuation">{</span>  
		<span class="token keyword">return</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xE7</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token punctuation">}</span>
</code></pre>
<hr>
<hr>
<h3 id="difference-bewteen-minimalntag-and-ntag">difference bewteen MinimalNtag and Ntag</h3>
<ul>
<li>MinimalNtag has <strong>tagType</strong> assertion to block unsupported product</li>
<li>MinimalNtag use <strong>readPages(0xE8)</strong>  to fetch configuration, and use getPages(0xEC) to fetch session</li>
<li>Ntag use <strong>read(Register.Configuration)</strong> to fetch configuration, and use <strong>read(Register.Session/Register.SessionPlus)</strong> to fetch session.</li>
<li>Ntag has different sector settings between different product.</li>
</ul>
<hr>
<h3 id="sr_offset">SR_Offset</h3>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">public</span> <span class="token keyword">enum</span> SR_Offset <span class="token punctuation">{</span>  
		<span class="token function">NC_REG</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
		<span class="token function">LAST_NDEF_PAGE</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x01</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
		<span class="token function">SM_REG</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x02</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
		<span class="token function">WDT_LS</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x03</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 		
		<span class="token function">WDT_MS</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x04</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
		<span class="token function">I2C_CLOCK_STR</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x05</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
		<span class="token function">NS_REG</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x06</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
		<span class="token function">FIXED</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x07</span><span class="token punctuation">)</span><span class="token punctuation">;</span>   
	<span class="token punctuation">}</span>
</code></pre>
</div>
</body>

</html>
