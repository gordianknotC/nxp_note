---


---

<h1 id="authenticate-and-protect">Authenticate And Protect</h1>
<h2 id="authenticate">Authenticate</h2>
<h3 id="ntag_i2c_command">Ntag_I2C_Command</h3>
<h4 id="authenticateplus">authenticatePlus</h4>
<p>Authenticate using PWD_AUTH command against NTAG I2C Plus product.</p>
<ul>
<li>
<h4 id="describe-writing-process-in-brief">describe writing process in brief</h4>
<ul>
<li>params
<ul>
<li><strong>byte[]</strong> pwd
<ul>
<li>4Byte password to authenticate with</li>
</ul>
</li>
</ul>
</li>
<li>check if its an <strong>Ntag_I2C_Plus</strong> product since only <strong>Plus</strong> support auth operation.</li>
<li>read auth data by using reader.<strong>pwdAuth</strong>
<h5 id="referenced-in">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>constructor</li>
<li>Auth</li>
<li>SRAMSpeedtestTask / doInBackground</li>
<li>EEPROMSpeedtestTask / doInBackground</li>
</ul>
</li>
<li>Ntag_I2C_Command
<ul>
<li>writeSRAMBlock</li>
<li>readSRAMBlock</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Override</span>  
<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">authenticatePlus</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> pwd<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> NotPlusTagException <span class="token punctuation">{</span>  
  <span class="token keyword">if</span><span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus <span class="token operator">&amp;&amp;</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
    <span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">NotPlusTagException</span><span class="token punctuation">(</span><span class="token string">"Auth Operations are not supported by non NTAG I2C PLUS products"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
  <span class="token punctuation">}</span>  
  <span class="token keyword">return</span> reader<span class="token punctuation">.</span><span class="token function">pwdAuth</span><span class="token punctuation">(</span>pwd<span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
<h3 id="minimalntag_i2c_command">MinimalNtag_I2C_Command</h3>
<h4 id="authenticateplus-1">authenticatePlus</h4>
<p>Authenticate using PWD_AUTH command against NTAG I2C Plus product.</p>
<ul>
<li>
<h4 id="describe-writing-process-in-brief-1">describe writing process in brief</h4>
<ul>
<li>params
<ul>
<li><strong>byte[]</strong> pwd
<ul>
<li>4Byte password to authenticate with</li>
</ul>
</li>
</ul>
</li>
<li>check if its an <strong>Ntag_I2C_Plus</strong> product since only <strong>Plus</strong> support auth operation.</li>
<li>perform auth operation by using <strong>mfu.transceive</strong>
<h5 id="referenced-in-1">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>constructor</li>
<li>Auth</li>
<li>SRAMSpeedtestTask / doInBackground</li>
<li>EEPROMSpeedtestTask / doInBackground</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Override</span>  
<span class="token keyword">public</span> <span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token function">authenticatePlus</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> pwd<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> NotPlusTagException <span class="token punctuation">{</span>  
	<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus <span class="token operator">&amp;&amp;</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
		<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">NotPlusTagException</span><span class="token punctuation">(</span><span class="token string">"Auth Operations are not supported by non NTAG I2C PLUS products"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>  
	<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> command <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">5</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x1B</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	command<span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span> <span class="token operator">=</span> pwd<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  
	<span class="token keyword">return</span> mfu<span class="token punctuation">.</span><span class="token function">transceive</span><span class="token punctuation">(</span>command<span class="token punctuation">)</span><span class="token punctuation">;</span>  
<span class="token punctuation">}</span>
</code></pre>
<h2 id="protect">Protect</h2>
<h3 id="ntag_i2c_command-1">Ntag_I2C_Command</h3>
<h4 id="protectplus">protectPlus</h4>
<p>Protect NTAG I2C Plus product memory map.</p>
<ul>
<li>
<h4 id="describe-writing-process-in-brief-2">describe writing process in brief</h4>
<ul>
<li>
<p>params</p>
<ul>
<li><strong>byte[]</strong> pwd
<ul>
<li>4Byte password to authenticate with</li>
</ul>
</li>
<li><strong>byte</strong> staretAddr
<ul>
<li>Page to lock the memory from</li>
</ul>
</li>
</ul>
</li>
<li>
<p>check if it’s an <strong>Ntag_I2C_Plus</strong> product, since auth operation not supported by non Ntag_I2C_Plus</p>
</li>
<li>
<p>reader.<strong>SectorSelect</strong> 0</p>
</li>
<li>
<p>set password indicated by user - using reader.<strong>write</strong>(pwd, Register.PWD)</p>
</li>
<li>
<p>write access configuration</p>
</li>
<li>
<p>write pt (pass thru) configuration</p>
</li>
<li>
<p>write auth0 configuration</p>
<h5 id="referenced-in-2">referenced in</h5>
<ul>
<li>Ntag_I2C_Demo
<ul>
<li>Auth</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>
<pre class=" language-java"><code class="prism  language-java">	<span class="token annotation punctuation">@Override</span>  
	<span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">protectPlus</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> pwd<span class="token punctuation">,</span> <span class="token keyword">byte</span> startAddr<span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> FormatException<span class="token punctuation">,</span> NotPlusTagException <span class="token punctuation">{</span>  
		<span class="token keyword">byte</span><span class="token punctuation">[</span><span class="token punctuation">]</span> data <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">byte</span><span class="token punctuation">[</span><span class="token number">4</span><span class="token punctuation">]</span><span class="token punctuation">;</span>  

		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_1k_Plus 
		<span class="token operator">&amp;&amp;</span> <span class="token function">getProduct</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> Prod<span class="token punctuation">.</span>NTAG_I2C_2k_Plus<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
			<span class="token keyword">throw</span> <span class="token keyword">new</span> <span class="token class-name">NotPlusTagException</span><span class="token punctuation">(</span><span class="token string">"Auth Operations are not supported by non NTAG I2C PLUS products"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
		<span class="token punctuation">}</span>  
		reader<span class="token punctuation">.</span><span class="token function">SectorSelect</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Set the password indicated by the user  </span>
		reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>pwd<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>PWD<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token keyword">byte</span> access <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token keyword">byte</span><span class="token punctuation">)</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span> authLimit <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span> <span class="token comment">// Don't limit the number of auth attempts  </span>

		access <span class="token operator">^=</span> <span class="token number">1</span> <span class="token operator">&lt;&lt;</span> Access_Offset<span class="token punctuation">.</span>NFC_PROT<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// NFC_Prot  </span>
		access <span class="token operator">^=</span> <span class="token number">0</span> <span class="token operator">&lt;&lt;</span> Access_Offset<span class="token punctuation">.</span>NFC_DIS_SEC1<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// NFC_DIS_SEC1  </span>
		access <span class="token operator">|=</span> authLimit <span class="token operator">&lt;&lt;</span> Access_Offset<span class="token punctuation">.</span>AUTH_LIM<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// AUTHLIM  </span>

		<span class="token comment">// Write the ACCESS configuration  data[0] = access;  </span>
		data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>ACCESS<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token keyword">byte</span> ptI2C <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		<span class="token keyword">byte</span> i2CProt <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  

		ptI2C <span class="token operator">^=</span> <span class="token number">0</span> <span class="token operator">&lt;&lt;</span> PT_I2C_Offset<span class="token punctuation">.</span>K2_PROT<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// 2K Prot  </span>
		ptI2C <span class="token operator">^=</span> <span class="token number">1</span> <span class="token operator">&lt;&lt;</span> PT_I2C_Offset<span class="token punctuation">.</span>SRAM_PROT<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// SRAM Prot  </span>
		ptI2C <span class="token operator">|=</span> i2CProt <span class="token operator">&lt;&lt;</span> PT_I2C_Offset<span class="token punctuation">.</span>I2C_PROT<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// I2C Prot  </span>

		<span class="token comment">// Write the PT_I2C configuration  data[0] = ptI2C;  </span>
		data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>PT_I2C<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  

		<span class="token comment">// Write the AUTH0 lock starting page  </span>
		data<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">2</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token number">0x00</span><span class="token punctuation">;</span>  
		data<span class="token punctuation">[</span><span class="token number">3</span><span class="token punctuation">]</span> <span class="token operator">=</span> startAddr<span class="token punctuation">;</span>  
		reader<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>data<span class="token punctuation">,</span> Register<span class="token punctuation">.</span>AUTH0<span class="token punctuation">.</span><span class="token function">getValue</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
	<span class="token punctuation">}</span>
</code></pre>
<h3 id="minimalntag_i2c_command-1">MinimalNtag_I2C_Command</h3>
<p>almost the same as Ntag_I2C, except using mfu.writePage</p>
<h2 id="unprotect">Unprotect</h2>
<p>behaviors almost the same as method of protect referred above.</p>
<h3 id="ntag_i2c_command-2">Ntag_I2C_Command</h3>
<h3 id="minimalntag_i2c_command-2">MinimalNtag_I2C_Command</h3>

