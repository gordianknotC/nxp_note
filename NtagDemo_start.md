<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>NtagDemo_start</title>
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
  <div class="stackedit__html"><h2 id="how-demo-be-initialized--ntag_i2c-">How Demo be Initialized ( Ntag_I2C )</h2>
<ul>
<li>
<h5 id="in-authactivity">in AuthActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// This authentication is added in order to avoid authentication problems with old NFC Controllers  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			demo<span class="token punctuation">.</span><span class="token function">Auth</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> AuthStatus<span class="token punctuation">.</span>Protected_RW<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">if</span><span class="token punctuation">(</span>demo <span class="token operator">!=</span> null <span class="token operator">&amp;&amp;</span> demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token comment">// Launch the thread  </span>
			task <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">authTask</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			task<span class="token punctuation">.</span><span class="token function">execute</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="in-flashmemoryactivity">in FlashMemoryActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token keyword">try</span> <span class="token punctuation">{</span>  
			<span class="token keyword">if</span><span class="token punctuation">(</span>isAppFW<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				bytesToFlash <span class="token operator">=</span> <span class="token function">readFileAssets</span><span class="token punctuation">(</span>indexFW<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
				String path <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token punctuation">(</span>TextView<span class="token punctuation">)</span> <span class="token function">findViewById</span><span class="token punctuation">(</span>R<span class="token punctuation">.</span>id<span class="token punctuation">.</span>file_path<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">getText</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				bytesToFlash <span class="token operator">=</span> <span class="token function">readFileMemory</span><span class="token punctuation">(</span>path<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
		<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">IOException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

			<span class="token comment">// Set bytesToFlash to null so that the task is not started  </span>
			bytesToFlash <span class="token operator">=</span> null<span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	
		<span class="token keyword">if</span><span class="token punctuation">(</span>bytesToFlash <span class="token operator">==</span> null <span class="token operator">||</span> bytesToFlash<span class="token punctuation">.</span>length <span class="token operator">==</span> <span class="token number">0</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			Toast<span class="token punctuation">.</span><span class="token function">makeText</span><span class="token punctuation">(</span>mContext<span class="token punctuation">,</span> <span class="token string">"Error could not open File"</span><span class="token punctuation">,</span>  
			Toast<span class="token punctuation">.</span>LENGTH_SHORT<span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">show</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token keyword">return</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span>  
			<span class="token keyword">return</span><span class="token punctuation">;</span>  

		<span class="token comment">// Retrieve the Auth Status  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>getAuthStatus<span class="token punctuation">)</span>  
			MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">ObtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Flashing is only available when the tag is not protected  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Unprotected<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token comment">// Launch the thread  </span>
			task <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">flashTask</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			task<span class="token punctuation">.</span><span class="token function">execute</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			<span class="token function">showAuthDialog</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="mainactivity">MainActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">
</code></pre>
<ul>
<li>
<h5 id="registerconfigactivity">RegisterConfigActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span><span class="token keyword">final</span> Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	    demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> 
		    MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
		    MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span>  
			<span class="token keyword">return</span><span class="token punctuation">;</span>  

		<span class="token comment">// Calculate the Register Values according to what has been selected by  </span>
		<span class="token comment">// the user  calcConfiguration();  </span>

		<span class="token comment">// Retrieve the Auth Status  </span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>getAuthStatus <span class="token operator">==</span> <span class="token boolean">true</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">ObtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">if</span> <span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Unprotected<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Protected_W<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Protected_W_SRAM<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">try</span> <span class="token punctuation">{</span>  
				demo<span class="token punctuation">.</span><span class="token function">readWriteConfigRegister</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">CommandNotSupportedException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token keyword">new</span> <span class="token class-name">AlertDialog<span class="token punctuation">.</span>Builder</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span>  
					<span class="token punctuation">.</span><span class="token function">setMessage</span><span class="token punctuation">(</span><span class="token string">"This NFC device does not support the NFC Forum commands needed to access the config register"</span><span class="token punctuation">)</span>  
					<span class="token punctuation">.</span><span class="token function">setTitle</span><span class="token punctuation">(</span><span class="token string">"Command not supported"</span><span class="token punctuation">)</span>  
					<span class="token punctuation">.</span><span class="token function">setPositiveButton</span><span class="token punctuation">(</span><span class="token string">"OK"</span><span class="token punctuation">,</span>  
						<span class="token keyword">new</span> <span class="token class-name">DialogInterface<span class="token punctuation">.</span>OnClickListener</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
							<span class="token annotation punctuation">@Override</span>  
							<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onClick</span><span class="token punctuation">(</span>DialogInterface dialog<span class="token punctuation">,</span> <span class="token keyword">int</span> which<span class="token punctuation">)</span> <span class="token punctuation">{</span>  

							<span class="token punctuation">}</span>  
						<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">show</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token keyword">return</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
			layout_read<span class="token punctuation">.</span><span class="token function">setVisibility</span><span class="token punctuation">(</span>View<span class="token punctuation">.</span>GONE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			layout_buttons<span class="token punctuation">.</span><span class="token function">setVisibility</span><span class="token punctuation">(</span>View<span class="token punctuation">.</span>VISIBLE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			scroll_regs<span class="token punctuation">.</span><span class="token function">setVisibility</span><span class="token punctuation">(</span>View<span class="token punctuation">.</span>VISIBLE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			<span class="token function">showAuthDialog</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="registersessionactivity">RegisterSessionActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span><span class="token keyword">final</span> Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span>  
			<span class="token keyword">return</span><span class="token punctuation">;</span>  

		<span class="token comment">// Calculate the Register Values according to what has been selected by  </span>
		<span class="token comment">// the user  calcConfiguration();  </span>

		<span class="token comment">// Retrieve the Auth Status  </span>
		<span class="token keyword">if</span> <span class="token punctuation">(</span>getAuthStatus <span class="token operator">==</span> <span class="token boolean">true</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">ObtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">if</span> <span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Unprotected<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Protected_W<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Protected_W_SRAM<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">try</span> <span class="token punctuation">{</span>  
				demo<span class="token punctuation">.</span><span class="token function">readWriteConfigRegister</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">CommandNotSupportedException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
				<span class="token keyword">new</span> <span class="token class-name">AlertDialog<span class="token punctuation">.</span>Builder</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span>  
					<span class="token punctuation">.</span><span class="token function">setMessage</span><span class="token punctuation">(</span>  
					<span class="token string">"This NFC device does not support the NFC Forum commands needed to access the config register"</span><span class="token punctuation">)</span>  
					<span class="token punctuation">.</span><span class="token function">setTitle</span><span class="token punctuation">(</span><span class="token string">"Command not supported"</span><span class="token punctuation">)</span>  
					<span class="token punctuation">.</span><span class="token function">setPositiveButton</span><span class="token punctuation">(</span><span class="token string">"OK"</span><span class="token punctuation">,</span> <span class="token keyword">new</span> <span class="token class-name">DialogInterface<span class="token punctuation">.</span>OnClickListener</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
						<span class="token annotation punctuation">@Override</span>  
						<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">onClick</span><span class="token punctuation">(</span>DialogInterface dialog<span class="token punctuation">,</span>  
						<span class="token keyword">int</span> which<span class="token punctuation">)</span> <span class="token punctuation">{</span>  

						<span class="token punctuation">}</span>  
					<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">show</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
				<span class="token keyword">return</span><span class="token punctuation">;</span>  
			<span class="token punctuation">}</span>  
			layout_read<span class="token punctuation">.</span><span class="token function">setVisibility</span><span class="token punctuation">(</span>View<span class="token punctuation">.</span>GONE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			layout_buttons<span class="token punctuation">.</span><span class="token function">setVisibility</span><span class="token punctuation">(</span>View<span class="token punctuation">.</span>VISIBLE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
			scroll_regs<span class="token punctuation">.</span><span class="token function">setVisibility</span><span class="token punctuation">(</span>View<span class="token punctuation">.</span>VISIBLE<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			<span class="token function">showAuthDialog</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="readmemoryactivity">ReadMemoryActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// Complete the task in a new thread in order to be able to show the dialog  </span>
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">return</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token comment">// Retrieve the Auth Status  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">ObtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Unprotected<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Protected_W<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Protected_W_SRAM<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// Launch the thread  </span>
			<span class="token keyword">new</span> <span class="token class-name">readTask</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">execute</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			<span class="token function">showAuthDialog</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>
<h5 id="versionactivity">VersionActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">
</code></pre>
<ul>
<li>
<h5 id="resetmemeoryactivity">ResetMemeoryActivity</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token keyword">private</span> <span class="token keyword">void</span> <span class="token function">startDemo</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">,</span> <span class="token keyword">boolean</span> getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		demo <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Ntag_I2C_Demo</span><span class="token punctuation">(</span>tag<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">,</span> 
			MainActivity<span class="token punctuation">.</span><span class="token function">getPassword</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span>  
			MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token operator">!</span>demo<span class="token punctuation">.</span><span class="token function">isReady</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">return</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token comment">// Retrieve the Auth Status  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>getAuthStatus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			MainActivity<span class="token punctuation">.</span><span class="token function">setAuthStatus</span><span class="token punctuation">(</span>demo<span class="token punctuation">.</span><span class="token function">ObtainAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  

		<span class="token comment">// Demo is only available when the tag is not protected  </span>
		<span class="token keyword">if</span><span class="token punctuation">(</span>MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Disabled<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Unprotected<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
		<span class="token operator">||</span> MainActivity<span class="token punctuation">.</span><span class="token function">getAuthStatus</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> AuthStatus<span class="token punctuation">.</span>Authenticated<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// Launch the thread  </span>
			<span class="token keyword">new</span> <span class="token class-name">resetTask</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">execute</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
			<span class="token function">showAuthDialog</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
	<span class="token punctuation">}</span>
</code></pre>
</div>
</body>

</html>
