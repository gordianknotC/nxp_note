<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>waitForReadAndWrite</title>
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
  <div class="stackedit__html"><h2 id="wait-for-read-and-write">Wait for Read and Write</h2>
<ul>
<li>
<h3 id="in-ntag_i2c_command--minimalntag_i2c_command">in Ntag_I2C_Command / MinimalNtag_I2C_Command</h3>
</li>
<li>waitForI2CWrite</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">waitforI2Cwrite</span><span class="token punctuation">(</span><span class="token keyword">int</span> timeoutMS<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> TimeoutException <span class="token punctuation">{</span>  
		<span class="token keyword">try</span> <span class="token punctuation">{</span>  
			Thread<span class="token punctuation">.</span><span class="token function">sleep</span><span class="token punctuation">(</span>timeoutMS<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">InterruptedException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">return</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<ul>
<li>waitForI2CRead</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">waitforI2Cread</span><span class="token punctuation">(</span><span class="token keyword">int</span> timeoutMS<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> TimeoutException <span class="token punctuation">{</span>  
		<span class="token keyword">try</span> <span class="token punctuation">{</span>  
			Thread<span class="token punctuation">.</span><span class="token function">sleep</span><span class="token punctuation">(</span>timeoutMS<span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">InterruptedException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		<span class="token keyword">return</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
</div>
</body>

</html>
