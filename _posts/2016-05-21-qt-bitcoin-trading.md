---
layout: post
title: Qt free bitcoin trading platform
categories: [trading, bots]
tags: [bots, charting]
repo: https://sourceforge.net/projects/bitcointrader/
permalink: qt-bitcoin-trading/
description:  Free bitcoin and altcoins trading platform released via bitcointalk forum. 
---


<p>First version was published on bitcointalk.org already some years ago. these days the code is hosted on sourceforge but updates still get published on bitcointalk.</p>

<p>Qt is available on Windows, Mac OS and Linux.</p>

<p><a href="https://bitcointalk.org/index.php?topic=201062.msg14943979#msg14943979">Link to May 2016 update</a></p>

<p><a href="http://centrabit.com/">Centrabit homepage</a></p>

<figure class="highlight"><pre><code class="language-python" data-lang="python"><span class="n">Description</span><span class="p">:</span>

<span class="n">Supported</span> <span class="n">exchanges</span><span class="p">:</span> <span class="n">OkCoin</span><span class="p">,</span> <span class="n">Bitfinex</span><span class="p">,</span> <span class="n">BTC</span><span class="o">-</span><span class="n">e</span><span class="p">,</span> <span class="n">Bitstamp</span><span class="p">,</span> <span class="n">goc</span><span class="o">.</span><span class="n">io</span><span class="p">,</span> <span class="n">Indacoin</span><span class="p">,</span> <span class="n">BTCChina</span><span class="p">,</span> <span class="n">Bitmarket</span><span class="o">.</span><span class="n">pl</span> <span class="ow">and</span> <span class="n">Bitcurex</span><span class="o">.</span>
<span class="n">This</span> <span class="n">software</span> <span class="n">helps</span> <span class="n">you</span> <span class="nb">open</span> <span class="ow">and</span> <span class="n">cancel</span> <span class="n">orders</span> <span class="n">very</span> <span class="n">fast</span><span class="o">.</span>
<span class="n">Real</span> <span class="n">time</span> <span class="n">data</span> <span class="n">monitoring</span><span class="o">.</span>
<span class="n">Developed</span> <span class="n">on</span> <span class="n">pure</span> <span class="n">Qt</span><span class="p">,</span> <span class="n">uses</span> <span class="n">OpenSSL</span><span class="o">.</span>

<span class="n">I</span> <span class="n">want</span> <span class="n">to</span> <span class="n">develop</span> <span class="n">this</span> <span class="n">Trader</span> <span class="n">App</span> <span class="n">so</span> <span class="n">that</span> <span class="n">it</span> <span class="n">can</span> <span class="n">be</span> <span class="n">configured</span> <span class="k">for</span> <span class="nb">any</span> <span class="n">rule</span> <span class="ow">and</span> <span class="n">strategy</span><span class="o">.</span>

<span class="n">And</span> <span class="n">now</span> <span class="n">it</span> <span class="n">supports</span> <span class="n">JL</span> <span class="n">Script</span><span class="p">,</span> <span class="n">advanced</span> <span class="n">strategy</span> <span class="n">script</span> <span class="n">language</span><span class="o">.</span> <span class="n">More</span> <span class="n">info</span> <span class="n">here</span> <span class="n">http</span><span class="p">:</span><span class="o">//</span><span class="n">forum</span><span class="o">.</span><span class="n">centrabit</span><span class="o">.</span><span class="n">com</span><span class="o">/</span><span class="n">viewforum</span><span class="o">.</span><span class="n">php</span><span class="err">?</span><span class="n">f</span><span class="o">=</span><span class="mi">3</span>

<span class="n">Next</span> <span class="n">ToDo</span><span class="p">:</span>
<span class="mi">1</span> <span class="p">)</span> <span class="n">Advanced</span> <span class="n">Charts</span>
<span class="mi">2</span> <span class="p">)</span> <span class="n">Add</span> <span class="n">support</span> <span class="n">to</span> <span class="n">monitor</span> <span class="n">many</span> <span class="n">exchanges</span> <span class="ow">and</span> <span class="n">currencies</span> <span class="n">at</span> <span class="n">the</span> <span class="n">same</span> <span class="n">time</span>
<span class="mi">3</span> <span class="p">)</span> <span class="n">Develop</span> <span class="n">server</span> <span class="n">to</span> <span class="n">collect</span> <span class="nb">all</span> <span class="n">ticker</span> <span class="ow">and</span> <span class="n">depth</span> <span class="n">data</span> <span class="n">to</span> <span class="n">provide</span> <span class="n">single</span> <span class="n">websocket</span> <span class="n">connection</span> <span class="k">for</span> <span class="n">realtime</span> <span class="n">data</span> <span class="n">updates</span>
<span class="mi">4</span> <span class="p">)</span> <span class="n">Make</span> <span class="n">floatable</span> <span class="n">interface</span>
<span class="mi">5</span> <span class="p">)</span> <span class="n">Allow</span> <span class="n">to</span> <span class="n">save</span> <span class="n">interface</span> <span class="n">settings</span> <span class="k">as</span> <span class="n">Workspace</span> <span class="n">profiles</span>
<span class="mi">6</span> <span class="p">)</span> <span class="n">Develop</span> <span class="n">mobile</span> <span class="n">application</span> <span class="n">to</span> <span class="n">provide</span> <span class="n">secure</span> <span class="n">remote</span> <span class="n">access</span> <span class="n">to</span> <span class="n">running</span> <span class="n">application</span> <span class="n">on</span> <span class="n">desktop</span>
<span class="mi">7</span> <span class="p">)</span> <span class="n">Add</span> <span class="n">plugins</span> <span class="n">support</span> <span class="n">to</span> <span class="n">allow</span> <span class="nb">all</span> <span class="n">developers</span> <span class="n">attach</span> <span class="nb">any</span> <span class="n">exchanges</span>

<span class="n">More</span> <span class="n">information</span> <span class="n">here</span><span class="p">:</span> <span class="n">http</span><span class="p">:</span><span class="o">//</span><span class="n">centrabit</span><span class="o">.</span><span class="n">com</span></code></pre></figure>


<br>
