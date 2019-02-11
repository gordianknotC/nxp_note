<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>transceive_command</title>
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
  <div class="stackedit__html"><h2 id="how-mfumifareultralight--nfca-transceive-command">How MFU(MifareUltralight) / NFCA transceive Command</h2>
<ul>
<li>
<h5 id="in-i2c_enabled_commands">in I2C_Enabled_Commands</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">static</span> I2C_Enabled_Commands <span class="token function">get</span><span class="token punctuation">(</span>Tag tag<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> InterruptedException <span class="token punctuation">{</span>  

<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
	nfca<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x60</span><span class="token punctuation">;</span> <span class="token comment">// GET_VERSION  </span>
	answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	prod <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Ntag_Get_Version</span><span class="token punctuation">(</span>answer<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">Get_Product</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>

	nfca<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xC2</span><span class="token punctuation">;</span> <span class="token comment">//SECTOR_SELECT  </span>
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xFF</span><span class="token punctuation">;</span>  
	answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
 
 
	mfu<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x60</span><span class="token punctuation">;</span> <span class="token comment">// GET_VERSION  </span>
	answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	prod <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Ntag_Get_Version</span><span class="token punctuation">(</span>answer<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">Get_Product</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	
  <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
  
	mfu<span class="token punctuation">.</span><span class="token function">connect</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token comment">// no exception is thrown so the phone can use the mfu.readPages  </span>
	<span class="token comment">// function // also check if: // - tag is from NXP (byte 0 == 0x04) // - CC corresponds to a NTAG I2C 1K  </span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span>answer<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x04</span> <span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">12</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xE1</span>  
	<span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">13</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x10</span> <span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">14</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x6D</span>  
	<span class="token operator">&amp;&amp;</span> answer<span class="token punctuation">[</span><span class="token number">15</span><span class="token punctuation">]</span> <span class="token operator">==</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token comment">// check if Config is readable (distinguish from NTAG216), if  </span>
		<span class="token comment">// not exception is thrown, and tag is not an // NTAG I2C 1k  answer = mfu.readPages(0xE8);  </span>
  
		<span class="token comment">// Try to read session registers to differentiate between standard and PLUS products  </span>
		<span class="token keyword">try</span> <span class="token punctuation">{</span>  
			answer <span class="token operator">=</span> mfu<span class="token punctuation">.</span><span class="token function">readPages</span><span class="token punctuation">(</span><span class="token number">0xEC</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
			mfu<span class="token punctuation">.</span><span class="token function">close</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
   <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
   
<span class="token punctuation">}</span>  
</code></pre>
<ul>
<li>
<h5 id="in-minintag_i2c_commands">in MiniNtag_I2C_Commands</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Override</span>  
<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">authenticatePlus</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> pwd<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> NotPlusTagException <span class="token punctuation">{</span>  
	<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus <span class="token operator">&amp;&amp;</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">NotPlusTagException</span><span class="token punctuation">(</span><span class="token string">"Auth Operations are not supported by non NTAG I2C PLUS products"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  
	<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">5</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x1B</span><span class="token punctuation">;</span>   <span class="token comment">// pwdauth</span>
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	<span class="token keyword">return</span> mfu<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
<h2 id="how-transceive-operates">How transceive operates</h2>
<ul>
<li>
<h5 id="sectorselect">SectorSelect</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xc2</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xff</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token comment">// pack two  </span>
	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> sector<span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">setTimeout</span><span class="token punctuation">(</span>sectorSelectTimout<span class="token punctuation">)</span><span class="token punctuation">;</span>
	<span class="token keyword">try</span> <span class="token punctuation">{</span>  
	  nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">IOException</span> e<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
	  e<span class="token punctuation">.</span><span class="token function">printStackTrace</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  
	nfca<span class="token punctuation">.</span><span class="token function">setTimeout</span><span class="token punctuation">(</span>timeout<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	currentSec <span class="token operator">=</span> sector<span class="token punctuation">;</span>
</code></pre>
<ul>
<li>
<h5 id="fast_write">fast_write</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xA6</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> startAddr<span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> endAddr<span class="token punctuation">;</span>
	System<span class="token punctuation">.</span><span class="token function">arraycopy</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> command<span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> data<span class="token punctuation">.</span>length<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">setTimeout</span><span class="token punctuation">(</span><span class="token number">500</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">setTimeout</span><span class="token punctuation">(</span>timeout<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<ul>
<li>
<h5 id="write">write</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">6</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0xA2</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> blockNr<span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span> <span class="token operator">=</span> data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">5</span><span class="token punctuation">]</span> <span class="token operator">=</span> data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
	nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<ul>
<li>
<h5 id="fast_read">fast_read</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x3A</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> startAddr<span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> endAddr<span class="token punctuation">;</span>
	nfca<span class="token punctuation">.</span><span class="token function">setTimeout</span><span class="token punctuation">(</span><span class="token number">500</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	nfca<span class="token punctuation">.</span><span class="token function">setTimeout</span><span class="token punctuation">(</span>timeout<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token keyword">return</span> answer<span class="token punctuation">;</span>
</code></pre>
<ul>
<li>
<h5 id="read">read</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x30</span><span class="token punctuation">;</span> <span class="token comment">// command:  </span>
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> blockNr<span class="token punctuation">;</span> <span class="token comment">// start page address  </span>
	answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token keyword">return</span> answer<span class="token punctuation">;</span>
</code></pre>
<ul>
<li>
<h5 id="getversion">getVersion</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x60</span><span class="token punctuation">;</span>  
	answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token keyword">return</span> answer<span class="token punctuation">;</span>
</code></pre>
<ul>
<li>
<h5 id="pwdauth">pwdAuth</h5>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">5</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x1B</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	answer <span class="token operator">=</span> nfca<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token keyword">return</span> answer<span class="token punctuation">;</span>
</code></pre>
</div>
</body>

</html>
