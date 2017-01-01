---
layout: page
title: Dummy data for backtesting
header: Dummy data for backtesting
group: navigation
permalink: datasets/
---


<ul>
  <li><a href="#bitcoin-futures-data">Bitcoin futures</a></li>
  <li><a href="#bitcoin-spot-market-data">Bitcoin spot data</a></li>
  <li><a href="#altcoin-markets-data">Altcoin market data</a></li>
  <li><a href="#data-for-sentiment-analysis">Sentiment</a></li>
</ul>

<hr />

<h2 id="bitcoin-futures-data">Bitcoin futures data</h2>

<p><strong>Futures premium data on OKCoin</strong></p>

<p>Updated hourly, free public JSON. There is a <em>parsed version without redundant data</em> that is compatible with Google’s free charting library, <a href="https://bitcoinfutures.info/data/premium/">see the spline chart here</a>.</p>

<p>Google charts compatible JSON: <a href="https://bitcoinfutures.info/data/premium.json">https://bitcoinfutures.info/data/premium.json</a></p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby">  <span class="p">[</span>
    <span class="p">[</span>
      <span class="p">{</span>
        <span class="no">Dataset</span><span class="p">:</span> <span class="s2">"OkCoin Premium on Weekly Futures"</span>
        <span class="p">},</span>
      <span class="p">{</span>
        <span class="no">Elements</span><span class="p">:</span> <span class="p">[</span>
          <span class="s2">"Timestamp"</span><span class="p">,</span>
          <span class="s2">"Spot Buy OKCoin"</span><span class="p">,</span>
          <span class="s2">"Weeklies Sell OKCoin"</span>
          <span class="p">]</span>
      <span class="p">},</span>
      <span class="p">{</span>
      <span class="no">Last</span> <span class="no">Updated</span><span class="p">:</span> <span class="s2">"2016-05-24"</span>
      <span class="p">}</span>
          <span class="p">],</span>
          <span class="p">[</span>
            <span class="mi">1463788868</span><span class="p">,</span>
            <span class="mi">445</span><span class="o">.</span><span class="mi">22</span><span class="p">,</span>
            <span class="mi">442</span>
          <span class="p">],</span>
          <span class="p">.</span><span class="nf">.</span><span class="o">.</span>
          </code></pre></figure>

<p>Then there is a full backup of all collected data since April 2016: It is available on firebase <a href="https://intense-inferno-1622.firebaseio.com/premium.json">https://intense-inferno-1622.firebaseio.com/premium.json</a>, the same data should be also available here: <a href="http://backtesting.bitcoinfutures.info/premium.json">http://backtesting.bitcoinfutures.info/premium.json</a>. Getting the data off firebase should be more stable. You will have to sanitize them, there is a huge overlap between the months.</p>

<p>The datasets are updated hourly.</p>

<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>

<script type="text/javascript">
      google.charts.load('current', {'packages':['corechart']});
      google.charts.setOnLoadCallback(drawChart);

      function drawChart() {


        var jsonData = $.ajax({
              url: "https://jsonp.afeld.me/?url=https://bitcoinfutures.info/data/premium.json",
              dataType: "json",
              async: false
              }).responseText;

        var data = google.visualization.arrayToDataTable($.parseJSON(jsonData), false);


        var options = {
          title: 'Futures Premium - Spot Buy vs. Weeklies Sell, OKCoin',
          curveType: 'function',
          legend: { position: 'bottom' },
         hAxis: { textPosition: 'none' }
        };

        var chart = new google.visualization.LineChart(document.getElementById('curve_chart'));

        chart.draw(data, options);
      }
</script>

<div id="curve_chart" style="width: 100%; min-height: 360px;"></div>

<h2 id="bitcoin-spot-market-data">Bitcoin spot market data</h2>

<p><strong>Coin Market Cap API</strong></p>

<p>Bitcoin and altcoins ticker and stats API. Available via https, updated minutely (?). It’s public JSON, easily parsed. API is well documented: <a href="http://coinmarketcap.com/api/">http://coinmarketcap.com/api/</a></p>

<p>It’s pretty cool for  simple website widgets.</p>

<p>Sample API response…</p>

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

<p>Sample JavaScript parser to show a line of data in a website’s header</p>

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

<h2 id="altcoin-markets-data">Altcoin markets data</h2>

<p><strong>Poloniex LAST price + percent change, ticker</strong></p>

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

<h2 id="data-for-sentiment-analysis">Data for sentiment analysis</h2>

<p><strong>Reddit /r/bitcoinmarkets daily thread top level comments</strong></p>

<p><a href="http://backtesting.bitcoinfutures.info/daily.json">http://backtesting.bitcoinfutures.info/daily.json</a></p>

<p>Updated daily</p>

<p>JSON public endpoint (<em>normally, Reddit requires authorization and limits all bots and scripts</em>)</p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby"><span class="p">{</span>
  <span class="ss">query: </span><span class="s2">"Daily Discussion 2016-05-24"</span><span class="p">,</span>
  <span class="ss">data: </span><span class="p">[</span>
    <span class="p">{</span>
      <span class="ss">name: </span><span class="s2">"t1_d3hgva4"</span><span class="p">,</span>
      <span class="ss">id: </span><span class="s2">"d3hgva4"</span><span class="p">,</span>
      <span class="ss">parent_id: </span><span class="s2">"t3_4ks0u8"</span><span class="p">,</span>
      <span class="p">.</span><span class="nf">.</span><span class="o">.</span></code></pre></figure>

<p><strong>Reddit selftext posts mentioning bitcoin exchanges</strong></p>

<p>Updated daily</p>

<p>JSON public endpoint (might be off or late at times)</p>

<p><a href="http://backtesting.bitcoinfutures.info/kraken.json">http://backtesting.bitcoinfutures.info/kraken.json</a></p>

<p><a href="http://backtesting.bitcoinfutures.info/bitstamp.json">http://backtesting.bitcoinfutures.info/bitstamp.json</a></p>

<p>… you get the system</p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby"><span class="p">{</span>
  <span class="ss">query: </span><span class="s2">"okcoin - 2016-05-24"</span><span class="p">,</span>
  <span class="ss">data: </span><span class="p">[</span>
    <span class="p">{</span>
      <span class="ss">title: </span><span class="s2">"OK COIN traders holding market down"</span><span class="p">,</span>
      <span class="ss">author: </span><span class="s2">"vroomDotClub"</span><span class="p">,</span>
      <span class="ss">permalink: </span><span class="s2">"/r/BitcoinMarkets/comments/4ipr3w/ok_coin_traders_holding_market_down/"</span><span class="p">,</span>
      <span class="ss">selftext: </span><span class="s2">"I've been watching markets side by side .. bstamp okcoin houbi etc .. seems as if the rest want to rise but OK coin keeps on slamming. Any thoughts on this anomaly? "</span>
    <span class="p">},</span>
    <span class="p">.</span><span class="nf">.</span><span class="o">.</span></code></pre></figure>
