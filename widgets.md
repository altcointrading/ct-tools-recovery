---
layout: page
title: Tickets and API endpoints with data for widgets
header: Tickets and API endpoints with data for widgets
group: navigation
permalink: widgets/
---



<p>You can also capture the data at an interval that suits you and use them for analysis or backtesting later once you’ve collected enough. Exchanges never provide too much of historical data and most of the repos or APIs here are free. They will be shut down one day, you should start collecting your own data if you can. A very easy way to do that is offered by <a href="https://firebase.com">Firebase</a> for instance.</p>

<hr />

<p><img src="/img/cmc-widget.jpeg" alt="coin market cap widget" /></p>

<h2 id="coin-market-cap-api">Coin Market Cap API</h2>

<p>Bitcoin and altcoins ticker and stats API. Available via https, updated minutely (?). It’s public JSON, easily parsed. API is well documented: <a href="http://coinmarketcap.com/api/">http://coinmarketcap.com/api/</a></p>

<p>It’s pretty cool for  simple website widgets - see image above. This is the code that does it:</p>

<p>The JSON API response</p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby">  <span class="p">[</span>
    <span class="p">{</span>
      <span class="ss">id: </span><span class="s2">"bitcoin"</span><span class="p">,</span>
      <span class="ss">name: </span><span class="s2">"Bitcoin"</span><span class="p">,</span>
      <span class="ss">symbol: </span><span class="s2">"BTC"</span><span class="p">,</span>
      <span class="ss">rank: </span><span class="mi">1</span><span class="p">,</span>
      <span class="ss">price_usd: </span><span class="mi">448</span><span class="o">.</span><span class="mi">529</span><span class="p">,</span>
      <span class="mi">24</span><span class="ss">h_volume_usd: </span><span class="mi">61357200</span><span class="p">,</span>
      <span class="ss">market_cap_usd: </span><span class="mi">6990918766</span><span class="p">,</span>
      <span class="ss">available_supply: </span><span class="mi">15586325</span><span class="p">,</span>
      <span class="ss">total_supply: </span><span class="mi">15586325</span><span class="p">,</span>
      <span class="ss">percent_change_1h: </span><span class="mi">0</span><span class="o">.</span><span class="mo">05</span><span class="p">,</span>
      <span class="ss">percent_change_24h: </span><span class="mi">0</span><span class="o">.</span><span class="mi">23</span><span class="p">,</span>
      <span class="ss">percent_change_7d: </span><span class="o">-</span><span class="mi">1</span><span class="o">.</span><span class="mi">13</span>
    <span class="p">}</span>
  <span class="p">]</span></code></pre></figure>

<p>JavaScript parser to show a line of data in a website’s header</p>

<figure class="highlight"><pre><code class="language-javascript" data-lang="javascript"><span class="kd">function</span> <span class="nx">loadJSON</span><span class="p">(</span><span class="nx">callback</span><span class="p">)</span> <span class="p">{</span>
  <span class="kd">var</span> <span class="nx">xobj</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">XMLHttpRequest</span><span class="p">();</span>
  <span class="nx">xobj</span><span class="p">.</span><span class="nx">overrideMimeType</span><span class="p">(</span><span class="s2">"application/json"</span><span class="p">);</span>
  <span class="nx">xobj</span><span class="p">.</span><span class="nx">open</span><span class="p">(</span><span class="s1">'GET'</span><span class="p">,</span> <span class="s1">'https://jsonp.afeld.me/?url=https://api.coinmarketcap.com/v1/ticker/bitcoin/'</span><span class="p">,</span> <span class="kc">true</span><span class="p">);</span>
  <span class="nx">xobj</span><span class="p">.</span><span class="nx">onreadystatechange</span> <span class="o">=</span> <span class="kd">function</span> <span class="p">()</span> <span class="p">{</span>
   <span class="k">if</span> <span class="p">(</span><span class="nx">xobj</span><span class="p">.</span><span class="nx">readyState</span> <span class="o">==</span> <span class="mi">4</span> <span class="o">&amp;&amp;</span> <span class="nx">xobj</span><span class="p">.</span><span class="nx">status</span> <span class="o">==</span> <span class="s2">"200"</span><span class="p">)</span> <span class="p">{</span>
     <span class="nx">callback</span><span class="p">(</span><span class="nx">xobj</span><span class="p">.</span><span class="nx">responseText</span><span class="p">);</span>
   <span class="p">}</span>
<span class="p">};</span>
  <span class="nx">xobj</span><span class="p">.</span><span class="nx">send</span><span class="p">(</span><span class="kc">null</span><span class="p">);</span>
<span class="p">}</span>
<span class="kd">function</span> <span class="nx">init</span><span class="p">()</span> <span class="p">{</span>
<span class="nx">loadJSON</span><span class="p">(</span><span class="kd">function</span><span class="p">(</span><span class="nx">response</span><span class="p">)</span> <span class="p">{</span>
  <span class="kd">var</span> <span class="nx">actual_JSON</span> <span class="o">=</span> <span class="nx">JSON</span><span class="p">.</span><span class="nx">parse</span><span class="p">(</span><span class="nx">response</span><span class="p">);</span>
  <span class="nb">document</span><span class="p">.</span><span class="nx">getElementById</span><span class="p">(</span><span class="s2">"topbar"</span><span class="p">).</span><span class="nx">innerHTML</span> <span class="o">=</span>
    <span class="s2">"&lt;small&gt;BTCUSD: "</span> <span class="o">+</span> <span class="nx">actual_JSON</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="s2">"price_usd"</span><span class="p">]</span>
    <span class="o">+</span> <span class="s2">"&lt;em&gt; | Change 1h: "</span> <span class="o">+</span>  <span class="nx">actual_JSON</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="s2">"percent_change_1h"</span><span class="p">]</span> <span class="o">+</span> <span class="s2">"%"</span>
    <span class="o">+</span> <span class="s2">" | Change 24h: "</span> <span class="o">+</span>  <span class="nx">actual_JSON</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="s2">"percent_change_24h"</span><span class="p">]</span> <span class="o">+</span> <span class="s2">"%"</span>
    <span class="o">+</span> <span class="s2">" | Change 7d: "</span> <span class="o">+</span>  <span class="nx">actual_JSON</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="s2">"percent_change_7d"</span><span class="p">]</span> <span class="o">+</span> <span class="s2">"%&lt;/em&gt;&lt;/small&gt;"</span><span class="p">;</span>
    <span class="p">});</span>
<span class="p">}</span>

<span class="nx">init</span><span class="p">();</span></code></pre></figure>

<h2 id="poloniex-altcoin-price-ticker">Poloniex altcoin price ticker</h2>

<p>Updated minutely</p>

<p>No access limit</p>

<p>JSON public endpoint</p>

<p><a href="http://api.altcoinwisdom.com/ticker.json">http://api.altcoinwisdom.com/ticker.json</a></p>

<p>For separate coin pair tickets see URIs at <a href="http://api.altcoinwisdom.com/">http://api.altcoinwisdom.com/</a>.</p>

<p>Also as HTML widget: <a href="https://jsonp.afeld.me/?url=http://api.altcoinwisdom.com/widget.html">black letters on white</a> or <a href="https://jsonp.afeld.me/?url=http://api.altcoinwisdom.com/widget-white.html">white letters on transparent background</a></p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby"><span class="p">[</span>
<span class="s2">"0.02797000"</span><span class="p">,</span>
<span class="o">-</span><span class="mi">8</span>
<span class="p">]</span></code></pre></figure>

<h2 id="multiple-charts-on-a-single-screen">Multiple charts on a single screen</h2>

<p><a href="http://altcointrading.github.io/">http://altcointrading.github.io/</a></p>

<p>The site only has the HTML connected to OkCoin futures api (only latest data, no historical), some altcoin price widget and two tradingview charts embedded next to each other. In this case it’s BTC price in USD and Yuan which makes it easier to observe if a breakout happened in the east but not yet on the west.</p>

<p>If you are able to edit HTML you can embed any chart you want.</p>

<p>If your computer has enough RAM you can then use this page as a GNOME desktop widget:</p>

<figure class="highlight"><pre><code class="language-python" data-lang="python"><span class="kn">from</span> <span class="nn">gi.repository</span> <span class="kn">import</span> <span class="n">WebKit</span><span class="p">,</span> <span class="n">Gtk</span><span class="p">,</span> <span class="n">Gdk</span><span class="p">,</span> <span class="n">Gio</span><span class="p">,</span> <span class="n">GLib</span>
<span class="kn">import</span> <span class="nn">signal</span><span class="o">,</span> <span class="nn">os</span>

<span class="k">class</span> <span class="nc">MainWin</span><span class="p">(</span><span class="n">Gtk</span><span class="o">.</span><span class="n">Window</span><span class="p">):</span>
    <span class="k">def</span> <span class="nf">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="n">Gtk</span><span class="o">.</span><span class="n">Window</span><span class="o">.</span><span class="n">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">skip_pager_hint</span><span class="o">=</span><span class="bp">True</span><span class="p">,</span> <span class="n">skip_taskbar_hint</span><span class="o">=</span><span class="bp">True</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_wmclass</span><span class="p">(</span><span class="s">"sildesktopwidget"</span><span class="p">,</span><span class="s">"sildesktopwidget"</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_type_hint</span><span class="p">(</span><span class="n">Gdk</span><span class="o">.</span><span class="n">WindowTypeHint</span><span class="o">.</span><span class="n">DOCK</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_size_request</span><span class="p">(</span><span class="mi">1120</span><span class="p">,</span><span class="mi">650</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_keep_below</span><span class="p">(</span><span class="bp">True</span><span class="p">)</span>

        <span class="c">#screen properties</span>
        <span class="n">screen</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">get_screen</span><span class="p">()</span>
        <span class="n">rgba</span> <span class="o">=</span> <span class="n">screen</span><span class="o">.</span><span class="n">get_rgba_visual</span><span class="p">()</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_visual</span><span class="p">(</span><span class="n">rgba</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">override_background_color</span><span class="p">(</span><span class="n">Gtk</span><span class="o">.</span><span class="n">StateFlags</span><span class="o">.</span><span class="n">NORMAL</span><span class="p">,</span> <span class="n">Gdk</span><span class="o">.</span><span class="n">RGBA</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">0</span><span class="p">,</span><span class="mi">0</span><span class="p">,</span><span class="mi">0</span><span class="p">))</span>

        <span class="c">#Add all the parts</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">view</span> <span class="o">=</span> <span class="n">WebKit</span><span class="o">.</span><span class="n">WebView</span><span class="p">()</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">view</span><span class="o">.</span><span class="n">set_transparent</span><span class="p">(</span><span class="bp">True</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">view</span><span class="o">.</span><span class="n">override_background_color</span><span class="p">(</span><span class="n">Gtk</span><span class="o">.</span><span class="n">StateFlags</span><span class="o">.</span><span class="n">NORMAL</span><span class="p">,</span> <span class="n">Gdk</span><span class="o">.</span><span class="n">RGBA</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span><span class="mi">0</span><span class="p">,</span><span class="mi">0</span><span class="p">,</span><span class="mi">0</span><span class="p">))</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">view</span><span class="o">.</span><span class="n">props</span><span class="o">.</span><span class="n">settings</span><span class="o">.</span><span class="n">props</span><span class="o">.</span><span class="n">enable_default_context_menu</span> <span class="o">=</span> <span class="bp">False</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">view</span><span class="o">.</span><span class="n">load_uri</span><span class="p">(</span><span class="s">"http://altcointrading.github.io/"</span><span class="p">)</span>

        <span class="n">box</span> <span class="o">=</span> <span class="n">Gtk</span><span class="o">.</span><span class="n">Box</span><span class="p">()</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">box</span><span class="p">)</span>
        <span class="n">box</span><span class="o">.</span><span class="n">pack_start</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">view</span><span class="p">,</span> <span class="bp">True</span><span class="p">,</span> <span class="bp">True</span><span class="p">,</span> <span class="mi">0</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">set_decorated</span><span class="p">(</span><span class="bp">False</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">connect</span><span class="p">(</span><span class="s">"destroy"</span><span class="p">,</span> <span class="k">lambda</span> <span class="n">q</span><span class="p">:</span> <span class="n">Gtk</span><span class="o">.</span><span class="n">main_quit</span><span class="p">())</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">show_all</span><span class="p">()</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">move</span><span class="p">(</span><span class="mi">10</span><span class="p">,</span><span class="mi">60</span><span class="p">)</span>

<span class="k">def</span> <span class="nf">refresh_file</span><span class="p">(</span><span class="o">*</span><span class="n">args</span><span class="p">):</span>
    <span class="k">print</span> <span class="n">args</span>
    <span class="n">mainwin</span><span class="o">.</span><span class="n">view</span><span class="o">.</span><span class="nb">reload</span><span class="p">()</span>

<span class="k">def</span> <span class="nf">file_changed</span><span class="p">(</span><span class="n">monitor</span><span class="p">,</span> <span class="nb">file</span><span class="p">,</span> <span class="n">unknown</span><span class="p">,</span> <span class="n">event</span><span class="p">):</span>
    <span class="c"># reload</span>
    <span class="n">GLib</span><span class="o">.</span><span class="n">timeout_add_seconds</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="n">refresh_file</span><span class="p">)</span>

<span class="k">if</span> <span class="n">__name__</span> <span class="o">==</span> <span class="s">'__main__'</span><span class="p">:</span>
    <span class="n">gio_file</span> <span class="o">=</span> <span class="n">Gio</span><span class="o">.</span><span class="n">File</span><span class="o">.</span><span class="n">new_for_path</span><span class="p">(</span><span class="s">"http://altcointrading.github.io/"</span><span class="p">)</span>
    <span class="n">monitor</span> <span class="o">=</span> <span class="n">gio_file</span><span class="o">.</span><span class="n">monitor_file</span><span class="p">(</span><span class="n">Gio</span><span class="o">.</span><span class="n">FileMonitorFlags</span><span class="o">.</span><span class="n">NONE</span><span class="p">,</span> <span class="bp">None</span><span class="p">)</span>
    <span class="n">monitor</span><span class="o">.</span><span class="n">connect</span><span class="p">(</span><span class="s">"changed"</span><span class="p">,</span> <span class="n">file_changed</span><span class="p">)</span>

    <span class="n">mainwin</span> <span class="o">=</span> <span class="n">MainWin</span><span class="p">()</span>
    <span class="n">signal</span><span class="o">.</span><span class="n">signal</span><span class="p">(</span><span class="n">signal</span><span class="o">.</span><span class="n">SIGINT</span><span class="p">,</span> <span class="n">signal</span><span class="o">.</span><span class="n">SIG_DFL</span><span class="p">)</span> <span class="c"># make ^c work</span>
    <span class="n">Gtk</span><span class="o">.</span><span class="n">main</span><span class="p">()</span></code></pre></figure>

<h2 id="okcoin-futures-html-widget">OkCoin Futures HTML widget</h2>

<p>You won’t see much since the writing is white but it’s latest OkCasino BTC futures data as a HTML widget. It’s updated every minute.</p>

<p><code class="highlighter-rouge">http://btc-jzgs.rhcloud.com/thisweek.html</code></p>

<p><code class="highlighter-rouge">http://btc-jzgs.rhcloud.com/nextweek.html</code></p>

<p><code class="highlighter-rouge">http://btc-jzgs.rhcloud.com/quarter.html</code></p>

<p>You can include the widget via an iframe. Use Proxy if you need HTTPS.</p>

<figure class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">"rite"</span><span class="nt">&gt;</span>
  <span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">"left"</span> <span class="na">style=</span><span class="s">"margin-left:10px"</span><span class="nt">&gt;</span>
    <span class="nt">&lt;iframe</span> <span class="na">src=</span><span class="s">"https://jsonp.afeld.me/?url=http://btc-jzgs.rhcloud.com/thisweek.html"</span> <span class="na">width=</span><span class="s">"240"</span> <span class="na">height=</span><span class="s">"230"</span> <span class="na">frameborder=</span><span class="s">"0"</span><span class="nt">&gt;&lt;/iframe&gt;</span>
  <span class="nt">&lt;/div&gt;</span>
  <span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">"left"</span><span class="nt">&gt;</span>
    <span class="nt">&lt;iframe</span> <span class="na">src=</span><span class="s">"https://jsonp.afeld.me/?url=http://btc-jzgs.rhcloud.com/nextweek.html"</span> <span class="na">width=</span><span class="s">"250"</span> <span class="na">height=</span><span class="s">"230"</span> <span class="na">frameborder=</span><span class="s">"0"</span><span class="nt">&gt;&lt;/iframe&gt;</span>
  <span class="nt">&lt;/div&gt;</span>
  <span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">"left"</span><span class="nt">&gt;</span>
   <span class="nt">&lt;iframe</span> <span class="na">src=</span><span class="s">"https://jsonp.afeld.me/?url=http://btc-jzgs.rhcloud.com/quarter.html"</span> <span class="na">width=</span><span class="s">"230"</span> <span class="na">height=</span><span class="s">"230"</span> <span class="na">frameborder=</span><span class="s">"0"</span><span class="nt">&gt;&lt;/iframe&gt;</span>
  <span class="nt">&lt;/div&gt;</span>
<span class="nt">&lt;/div&gt;</span></code></pre></figure>
