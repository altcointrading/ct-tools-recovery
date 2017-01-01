---
layout: page
title: Historical Data - Best days of the week to go long and short
header: Historical Data - Best days of the week to go long and short
group: navigation
permalink: trading-stats/
---


<p>Hover over each column for more information.</p>

<iframe src="https://jsonp.afeld.me/?url=https://bitcoinfutures.de/daily/bitfinex.html" style="border: 0;width: 100%;height: 250px;"></iframe>

<h2 id="what-it-is">What it is</h2>

<p>The chart is a product of a Ruby script which goes through daily BTCUSD data, finds the following day’s data to each datapoint (each datapoint is single day’s data). Then it looks at highs of the days - if on the day <em>zero</em> the high was lower than the high on day <em>one</em>, it makes day <em>zero</em> a good day to go long. In that case the script finds the day of the week of that date and gives a point to that day of the week.</p>

<p>It then does the same thing with lows: Day <em>zero</em> is a good day to go short if day <em>one</em> had a lower low.</p>

<p>The percentage is related to all occurrences of the respective day of the week in the initial dataset. It’s not meant to be dead accurate - it’s overgeneralized anyway but many might argue that the price might be driven by weekly futures on Chinese exchanges or maybe something else because there <strong>are</strong> days when it’s better to go long and days that are better for shorters - see tuesday - bad to go long + good to go short.</p>

<p>It is not meant to be your solely strategy though, and none of this is an ancouragement nor investment advice.</p>

<h2 id="how-to-embed-the-chart">How to embed the chart</h2>

<p>The chart above is embedded via iframe linked from a proxy. It is linked directly to the source that updates daily. If you want to feature the chart on your website here’s the snippet:</p>

<figure class="highlight"><pre><code class="language-html" data-lang="html"><span class="nt">&lt;iframe</span> <span class="na">src=</span><span class="s">"https://jsonp.afeld.me/?url=https://bitcoinfutures.de/daily/bitfinex.html"</span> <span class="na">style=</span><span class="s">"border: 0;width: 100%;height: 250px;"</span><span class="nt">&gt;&lt;/iframe&gt;</span></code></pre></figure>

<h2 id="actual-code">Actual code</h2>

<p>Ruby module that depends on the gems listed in the head of the file.</p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby"><span class="k">module</span> <span class="nn">Daily</span>

  <span class="kp">extend</span> <span class="nb">self</span>

  <span class="nb">require</span> <span class="s2">"net/http"</span>
  <span class="nb">require</span> <span class="s2">"uri"</span>
  <span class="nb">require</span> <span class="s2">"json"</span>
  <span class="nb">require</span> <span class="s1">'time'</span>
  <span class="nb">require</span> <span class="s1">'pp'</span>


  <span class="k">class</span> <span class="nc">Days</span>

    <span class="k">def</span> <span class="nf">initialize</span> <span class="n">uri</span>
      <span class="n">res</span> <span class="o">=</span> <span class="no">Net</span><span class="o">::</span><span class="no">HTTP</span><span class="p">.</span><span class="nf">get_response</span><span class="p">(</span><span class="no">URI</span><span class="p">.</span><span class="nf">parse</span><span class="p">(</span><span class="n">uri</span><span class="p">))</span>
      <span class="vi">@data</span> <span class="o">=</span> <span class="no">JSON</span><span class="p">.</span><span class="nf">parse</span><span class="p">(</span> <span class="n">res</span><span class="p">.</span><span class="nf">body</span> <span class="p">)[</span><span class="s1">'dataset'</span><span class="p">][</span><span class="s1">'data'</span><span class="p">]</span>
    <span class="k">end</span>

    <span class="k">def</span> <span class="nf">sorting</span>
      <span class="k">raise</span> <span class="no">NotImplementedError</span><span class="p">,</span> <span class="s2">"sorting method not implemented in </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
    <span class="k">end</span>
    <span class="k">def</span> <span class="nf">counter_yday</span>
      <span class="k">raise</span> <span class="no">NotImplementedError</span><span class="p">,</span> <span class="s2">"counter_yday method not implemented in </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
    <span class="k">end</span>
    <span class="k">def</span> <span class="nf">counter_tmr</span>
      <span class="k">raise</span> <span class="no">NotImplementedError</span><span class="p">,</span> <span class="s2">"counter_tmr method not implemented in </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
    <span class="k">end</span>
    <span class="k">def</span> <span class="nf">neighbors</span> <span class="n">data</span><span class="p">,</span> <span class="n">item</span>
      <span class="k">raise</span> <span class="no">NotImplementedError</span><span class="p">,</span> <span class="s2">"neighbors method not implemented in </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
    <span class="k">end</span>

  <span class="k">end</span><span class="c1">#days</span>

  <span class="k">class</span> <span class="nc">Finex</span> <span class="o">&lt;</span> <span class="no">Days</span>

    <span class="k">def</span> <span class="nf">sorting</span>
      <span class="c1"># finex no haz op cl</span>
      <span class="c1"># get hi and lo</span>
      <span class="c1"># ====</span>
      <span class="c1"># date hi lo</span>
      <span class="c1"># 0 1 2</span>
      <span class="n">datapoints</span> <span class="o">=</span> <span class="p">[]</span>
      <span class="vi">@data</span><span class="p">.</span><span class="nf">each</span> <span class="k">do</span> <span class="o">|</span><span class="n">item</span><span class="o">|</span>
       <span class="n">inner</span> <span class="o">=</span> <span class="p">[]</span>
        <span class="n">date</span> <span class="o">=</span> <span class="n">item</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span>
        <span class="n">hi</span> <span class="o">=</span> <span class="n">item</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span>
        <span class="n">lo</span> <span class="o">=</span> <span class="n">item</span><span class="p">[</span><span class="mi">2</span><span class="p">]</span>
        <span class="c1">#date = "2016-05-29"</span>
        <span class="n">realdate</span> <span class="o">=</span> <span class="no">Time</span><span class="p">.</span><span class="nf">parse</span><span class="p">(</span><span class="n">date</span><span class="p">)</span>
        <span class="c1">#dotw = realdate.strftime("%A").downcase</span>
        <span class="n">day</span> <span class="o">=</span> <span class="n">realdate</span><span class="p">.</span><span class="nf">day</span>
        <span class="n">month</span> <span class="o">=</span> <span class="n">realdate</span><span class="p">.</span><span class="nf">month</span>
        <span class="n">year</span> <span class="o">=</span> <span class="n">realdate</span><span class="p">.</span><span class="nf">year</span>
        <span class="n">dotw</span> <span class="o">=</span> <span class="no">Date</span><span class="p">.</span><span class="nf">new</span><span class="p">(</span><span class="n">year</span><span class="p">,</span><span class="n">month</span><span class="p">,</span><span class="n">day</span><span class="p">).</span><span class="nf">wday</span>
       <span class="n">inner</span><span class="p">.</span><span class="nf">push</span><span class="p">(</span><span class="n">day</span><span class="p">,</span> <span class="n">month</span><span class="p">,</span> <span class="n">year</span><span class="p">,</span> <span class="n">dotw</span><span class="p">,</span> <span class="n">hi</span><span class="p">,</span> <span class="n">lo</span> <span class="p">)</span>
       <span class="n">datapoints</span><span class="p">.</span><span class="nf">push</span><span class="p">(</span><span class="n">inner</span><span class="p">)</span>
      <span class="k">end</span>
      <span class="k">return</span> <span class="n">datapoints</span>
    <span class="k">end</span>

    <span class="k">def</span> <span class="nf">counter_yday</span>
      <span class="c1"># Sunday is 0</span>
      <span class="c1"># ==========</span>
      <span class="c1"># total counts</span>
      <span class="c1"># =============</span>
      <span class="n">tot_mo</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_tu</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_we</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_th</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_fr</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_sa</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_su</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">data</span> <span class="o">=</span> <span class="nb">self</span><span class="p">.</span><span class="nf">sorting</span>
        <span class="n">data</span><span class="p">.</span><span class="nf">uniq!</span>
        <span class="n">data</span> <span class="o">=</span> <span class="n">data</span><span class="p">.</span><span class="nf">map</span><span class="p">.</span><span class="nf">with_index</span><span class="p">(</span><span class="mi">0</span><span class="p">).</span><span class="nf">to_a</span>
        <span class="n">sets</span> <span class="o">=</span> <span class="p">[]</span>
      <span class="n">data</span><span class="p">.</span><span class="nf">each</span> <span class="k">do</span> <span class="o">|</span><span class="n">item</span><span class="o">|</span>
        <span class="n">sets</span><span class="p">.</span><span class="nf">push</span><span class="p">(</span> <span class="nb">self</span><span class="p">.</span><span class="nf">neighbors</span><span class="p">(</span><span class="n">data</span><span class="p">,</span><span class="n">item</span><span class="p">)</span> <span class="p">)</span>
        <span class="c1"># count days totals</span>
        <span class="n">dotw</span> <span class="o">=</span> <span class="n">item</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">3</span><span class="p">]</span>
          <span class="k">case</span> <span class="n">dotw</span>
          <span class="k">when</span> <span class="mi">0</span>
            <span class="n">tot_su</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">1</span>
            <span class="n">tot_mo</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">2</span>
            <span class="n">tot_tu</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">3</span>
            <span class="n">tot_we</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">4</span>
            <span class="n">tot_th</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">5</span>
            <span class="n">tot_fr</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">6</span>
            <span class="n">tot_sa</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">else</span>
            <span class="k">raise</span> <span class="no">StandardError</span><span class="p">,</span> <span class="s2">"Faulty reading of DOTW days counter at </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
          <span class="k">end</span><span class="c1">#case</span>
      <span class="k">end</span>
        <span class="n">sets</span><span class="p">.</span><span class="nf">pop</span>
        <span class="n">sets</span><span class="p">.</span><span class="nf">shift</span>
      <span class="c1">#pp sets</span>
      <span class="c1"># ===============</span>
      <span class="c1"># Actual Counter (TM)</span>
      <span class="c1"># == higher high than previous day:</span>
      <span class="n">hi_mo</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">hi_tu</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">hi_we</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">hi_th</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">hi_fr</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">hi_sa</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">hi_su</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="c1"># == lower low than previous day</span>
      <span class="n">lo_mo</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">lo_tu</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">lo_we</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">lo_th</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">lo_fr</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">lo_sa</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">lo_su</span> <span class="o">=</span> <span class="mi">0</span>

      <span class="n">sets</span><span class="p">.</span><span class="nf">each</span> <span class="k">do</span> <span class="o">|</span><span class="n">set</span><span class="o">|</span>
        <span class="c1"># highs: tday &gt; yday</span>
        <span class="k">if</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">4</span><span class="p">]</span><span class="o">&gt;</span><span class="n">set</span><span class="p">[</span><span class="mi">1</span><span class="p">][</span><span class="mi">4</span><span class="p">]</span>
          <span class="n">dotw_tday</span> <span class="o">=</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">3</span><span class="p">]</span>
          <span class="k">case</span> <span class="n">dotw_tday</span>
          <span class="k">when</span> <span class="mi">0</span>
            <span class="n">hi_su</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">1</span>
            <span class="n">hi_mo</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">2</span>
            <span class="n">hi_tu</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">3</span>
            <span class="n">hi_we</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">4</span>
            <span class="n">hi_th</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">5</span>
            <span class="n">hi_fr</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">6</span>
            <span class="n">hi_sa</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">else</span>
            <span class="k">raise</span> <span class="no">StandardError</span><span class="p">,</span> <span class="s2">"Faulty reading of DOTW highs at </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
          <span class="k">end</span><span class="c1">#case</span>
        <span class="k">end</span><span class="c1">#if</span>
        <span class="c1"># lows: tday &lt; yday</span>
        <span class="k">if</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">5</span><span class="p">]</span><span class="o">&lt;</span><span class="n">set</span><span class="p">[</span><span class="mi">1</span><span class="p">][</span><span class="mi">5</span><span class="p">]</span>
          <span class="n">dotw_tday</span> <span class="o">=</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">3</span><span class="p">]</span>
          <span class="k">case</span> <span class="n">dotw_tday</span>
          <span class="k">when</span> <span class="mi">0</span>
            <span class="n">lo_su</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">1</span>
            <span class="n">lo_mo</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">2</span>
            <span class="n">lo_tu</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">3</span>
            <span class="n">lo_we</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">4</span>
            <span class="n">lo_th</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">5</span>
            <span class="n">lo_fr</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">6</span>
            <span class="n">lo_sa</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">else</span>
            <span class="k">raise</span> <span class="no">StandardError</span><span class="p">,</span> <span class="s2">"Faulty reading of DOTW lows at </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
          <span class="k">end</span><span class="c1">#case</span>
        <span class="k">end</span><span class="c1">#if</span>
        <span class="c1">#if set[0][4]&lt;set[2][4] then p "tday lowr hi than tmr" end</span>
      <span class="k">end</span>
      <span class="nb">puts</span> <span class="s2">"Highs: </span><span class="si">#{</span><span class="n">hi_mo</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">hi_tu</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">hi_we</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">hi_th</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">hi_fr</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">hi_sa</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">hi_su</span><span class="si">}</span><span class="s2">"</span>
      <span class="nb">puts</span> <span class="s2">"Lows: </span><span class="si">#{</span><span class="n">lo_mo</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">lo_tu</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">lo_we</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">lo_th</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">lo_fr</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">lo_sa</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">lo_su</span><span class="si">}</span><span class="s2">"</span>
      <span class="nb">puts</span> <span class="s2">"Totals: </span><span class="si">#{</span><span class="n">tot_mo</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">tot_tu</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">tot_we</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">tot_th</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">tot_fr</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">tot_sa</span><span class="si">}</span><span class="s2">, </span><span class="si">#{</span><span class="n">tot_su</span><span class="si">}</span><span class="s2">"</span>

      <span class="n">percentages</span> <span class="o">=</span> <span class="p">[</span>
        <span class="p">[</span><span class="s2">"Last update </span><span class="si">#{</span><span class="no">Time</span><span class="p">.</span><span class="nf">now</span><span class="si">}</span><span class="s2">, [Day of the week, Grew , Fell]"</span><span class="p">],</span>
        <span class="p">[</span>
          <span class="p">[</span> <span class="s2">"Monday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">hi_mo</span><span class="o">/</span><span class="n">tot_mo</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">lo_mo</span><span class="o">/</span><span class="n">tot_mo</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Tuesday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">hi_tu</span><span class="o">/</span><span class="n">tot_tu</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">lo_tu</span><span class="o">/</span><span class="n">tot_tu</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Wednesday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">hi_we</span><span class="o">/</span><span class="n">tot_we</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">lo_we</span><span class="o">/</span><span class="n">tot_we</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Thursday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">hi_th</span><span class="o">/</span><span class="n">tot_th</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">lo_th</span><span class="o">/</span><span class="n">tot_th</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Friday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">hi_fr</span><span class="o">/</span><span class="n">tot_fr</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">lo_fr</span><span class="o">/</span><span class="n">tot_fr</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Saturday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">hi_sa</span><span class="o">/</span><span class="n">tot_sa</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">lo_sa</span><span class="o">/</span><span class="n">tot_sa</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Sunday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">hi_su</span><span class="o">/</span><span class="n">tot_su</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">lo_su</span><span class="o">/</span><span class="n">tot_su</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)]</span>
        <span class="p">]</span>
      <span class="p">]</span>
      <span class="nb">p</span> <span class="n">percentages</span>
      <span class="n">title</span> <span class="o">=</span> <span class="s2">"today-yday"</span>
        <span class="n">jf</span> <span class="o">=</span> <span class="nb">open</span><span class="p">(</span> <span class="s2">"</span><span class="si">#{</span><span class="n">title</span><span class="si">}</span><span class="s2">.json"</span><span class="p">,</span> <span class="s1">'w+'</span><span class="p">)</span>
        <span class="n">jf</span><span class="p">.</span><span class="nf">write</span><span class="p">(</span> <span class="n">percentages</span><span class="p">.</span><span class="nf">to_json</span> <span class="p">)</span>
        <span class="n">jf</span><span class="p">.</span><span class="nf">close</span>
      <span class="k">if</span> <span class="no">Date</span><span class="p">.</span><span class="nf">today</span><span class="p">.</span><span class="nf">wday</span><span class="o">==</span><span class="mi">0</span> <span class="k">then</span> <span class="nb">self</span><span class="p">.</span><span class="nf">oddjob</span><span class="p">(</span><span class="n">title</span><span class="p">,</span> <span class="n">percentages</span><span class="p">)</span> <span class="k">end</span>
      <span class="k">return</span> <span class="n">percentages</span>
    <span class="k">end</span>

    <span class="k">def</span> <span class="nf">counter_tmr</span>
      <span class="c1"># Sunday is 0</span>
      <span class="c1"># ==========</span>
      <span class="c1"># total counts</span>
      <span class="c1"># =============</span>
      <span class="n">tot_mo</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_tu</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_we</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_th</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_fr</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_sa</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">tot_su</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">data</span> <span class="o">=</span> <span class="nb">self</span><span class="p">.</span><span class="nf">sorting</span>
        <span class="n">data</span><span class="p">.</span><span class="nf">uniq!</span>
        <span class="n">data</span> <span class="o">=</span> <span class="n">data</span><span class="p">.</span><span class="nf">map</span><span class="p">.</span><span class="nf">with_index</span><span class="p">(</span><span class="mi">0</span><span class="p">).</span><span class="nf">to_a</span>
        <span class="n">sets</span> <span class="o">=</span> <span class="p">[]</span>
      <span class="n">data</span><span class="p">.</span><span class="nf">each</span> <span class="k">do</span> <span class="o">|</span><span class="n">item</span><span class="o">|</span>
        <span class="n">sets</span><span class="p">.</span><span class="nf">push</span><span class="p">(</span> <span class="nb">self</span><span class="p">.</span><span class="nf">neighbors</span><span class="p">(</span><span class="n">data</span><span class="p">,</span><span class="n">item</span><span class="p">)</span> <span class="p">)</span>
        <span class="c1"># count days totals</span>
        <span class="n">dotw</span> <span class="o">=</span> <span class="n">item</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">3</span><span class="p">]</span>
          <span class="k">case</span> <span class="n">dotw</span>
          <span class="k">when</span> <span class="mi">0</span>
            <span class="n">tot_su</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">1</span>
            <span class="n">tot_mo</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">2</span>
            <span class="n">tot_tu</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">3</span>
            <span class="n">tot_we</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">4</span>
            <span class="n">tot_th</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">5</span>
            <span class="n">tot_fr</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">6</span>
            <span class="n">tot_sa</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">else</span>
            <span class="k">raise</span> <span class="no">StandardError</span><span class="p">,</span> <span class="s2">"Faulty reading of DOTW days counter at </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
          <span class="k">end</span><span class="c1">#case</span>
      <span class="k">end</span>
        <span class="n">sets</span><span class="p">.</span><span class="nf">pop</span>
        <span class="n">sets</span><span class="p">.</span><span class="nf">shift</span>
      <span class="c1">#pp sets</span>
      <span class="c1"># ===============</span>
      <span class="c1"># Actual Counter (TM)</span>
        <span class="c1"># == day 1 higher high than day 0 = long on day 0:</span>
        <span class="n">l_mo</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">l_tu</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">l_we</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">l_th</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">l_fr</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">l_sa</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">l_su</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="c1"># == day 1 lower low than day 0 = short on day 0</span>
        <span class="n">s_mo</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">s_tu</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">s_we</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">s_th</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">s_fr</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">s_sa</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">s_su</span> <span class="o">=</span> <span class="mi">0</span>

      <span class="n">sets</span><span class="p">.</span><span class="nf">each</span> <span class="k">do</span> <span class="o">|</span><span class="n">set</span><span class="o">|</span>
        <span class="c1"># highs: hi tday &lt; hi tmr (long)</span>
        <span class="k">if</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">4</span><span class="p">]</span><span class="o">&lt;</span><span class="n">set</span><span class="p">[</span><span class="mi">2</span><span class="p">][</span><span class="mi">4</span><span class="p">]</span>
          <span class="n">dotw_tday</span> <span class="o">=</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">3</span><span class="p">]</span>
          <span class="k">case</span> <span class="n">dotw_tday</span>
          <span class="k">when</span> <span class="mi">0</span>
            <span class="n">l_su</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">1</span>
            <span class="n">l_mo</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">2</span>
            <span class="n">l_tu</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">3</span>
            <span class="n">l_we</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">4</span>
            <span class="n">l_th</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">5</span>
            <span class="n">l_fr</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">6</span>
            <span class="n">l_sa</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">else</span>
            <span class="k">raise</span> <span class="no">StandardError</span><span class="p">,</span> <span class="s2">"Faulty reading of DOTW highs at </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
          <span class="k">end</span><span class="c1">#case</span>
        <span class="k">end</span><span class="c1">#if</span>
        <span class="c1"># lows: low today &gt; low tmr (short)</span>
        <span class="k">if</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">5</span><span class="p">]</span><span class="o">&gt;</span><span class="n">set</span><span class="p">[</span><span class="mi">2</span><span class="p">][</span><span class="mi">5</span><span class="p">]</span>
          <span class="n">dotw_tday</span> <span class="o">=</span> <span class="n">set</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="mi">3</span><span class="p">]</span>
          <span class="k">case</span> <span class="n">dotw_tday</span>
          <span class="k">when</span> <span class="mi">0</span>
            <span class="n">s_su</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">1</span>
            <span class="n">s_mo</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">2</span>
            <span class="n">s_tu</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">3</span>
            <span class="n">s_we</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">4</span>
            <span class="n">s_th</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">5</span>
            <span class="n">s_fr</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">when</span> <span class="mi">6</span>
            <span class="n">s_sa</span> <span class="o">+=</span> <span class="mi">1</span>
          <span class="k">else</span>
            <span class="k">raise</span> <span class="no">StandardError</span><span class="p">,</span> <span class="s2">"Faulty reading of DOTW lows at </span><span class="si">#{</span><span class="nb">self</span><span class="p">.</span><span class="nf">class</span><span class="si">}</span><span class="s2">"</span>
          <span class="k">end</span><span class="c1">#case</span>
        <span class="k">end</span><span class="c1">#if</span>
        <span class="c1">#if set[0][4]&lt;set[2][4] then p "tday lowr hi than tmr" end</span>
      <span class="k">end</span>

      <span class="n">percentages</span> <span class="o">=</span> <span class="p">[</span>
        <span class="p">[</span><span class="s2">"Last update </span><span class="si">#{</span><span class="no">Time</span><span class="p">.</span><span class="nf">now</span><span class="si">}</span><span class="s2">, [Day of the week, To Long On That Day (%), To Short On That Day (%)]"</span><span class="p">],</span>
        <span class="p">[</span>
          <span class="p">[</span> <span class="s2">"Monday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">l_mo</span><span class="o">/</span><span class="n">tot_mo</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">s_mo</span><span class="o">/</span><span class="n">tot_mo</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Tuesday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">l_tu</span><span class="o">/</span><span class="n">tot_tu</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">s_tu</span><span class="o">/</span><span class="n">tot_tu</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Wednesday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">l_we</span><span class="o">/</span><span class="n">tot_we</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">s_we</span><span class="o">/</span><span class="n">tot_we</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Thursday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">l_th</span><span class="o">/</span><span class="n">tot_th</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">s_th</span><span class="o">/</span><span class="n">tot_th</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Friday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">l_fr</span><span class="o">/</span><span class="n">tot_fr</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">s_fr</span><span class="o">/</span><span class="n">tot_fr</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Saturday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">l_sa</span><span class="o">/</span><span class="n">tot_sa</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">s_sa</span><span class="o">/</span><span class="n">tot_sa</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)],</span>
          <span class="p">[</span> <span class="s2">"Sunday"</span><span class="p">,</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">l_su</span><span class="o">/</span><span class="n">tot_su</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">),</span> <span class="p">(</span><span class="mi">100</span><span class="o">.</span><span class="mo">00</span><span class="o">*</span><span class="n">s_su</span><span class="o">/</span><span class="n">tot_su</span><span class="p">).</span><span class="nf">round</span><span class="p">(</span><span class="mi">2</span><span class="p">)]</span>
        <span class="p">]</span>
      <span class="p">]</span>
      <span class="nb">p</span> <span class="n">percentages</span>
      <span class="n">title</span> <span class="o">=</span> <span class="s2">"today-tmr"</span>
        <span class="n">jf</span> <span class="o">=</span> <span class="nb">open</span><span class="p">(</span> <span class="s2">"</span><span class="si">#{</span><span class="n">title</span><span class="si">}</span><span class="s2">.json"</span><span class="p">,</span> <span class="s1">'w+'</span><span class="p">)</span>
        <span class="n">jf</span><span class="p">.</span><span class="nf">write</span><span class="p">(</span> <span class="n">percentages</span><span class="p">.</span><span class="nf">to_json</span> <span class="p">)</span>
        <span class="n">jf</span><span class="p">.</span><span class="nf">close</span>
      <span class="k">if</span> <span class="no">Date</span><span class="p">.</span><span class="nf">today</span><span class="p">.</span><span class="nf">wday</span><span class="o">==</span><span class="mi">0</span> <span class="k">then</span> <span class="nb">self</span><span class="p">.</span><span class="nf">oddjob</span><span class="p">(</span><span class="n">title</span><span class="p">,</span> <span class="n">percentages</span><span class="p">)</span> <span class="k">end</span>
      <span class="k">return</span> <span class="n">percentages</span>
    <span class="k">end</span>

    <span class="k">def</span> <span class="nf">neighbors</span> <span class="n">data</span><span class="p">,</span> <span class="n">item</span>
      <span class="n">prior</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">following</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="n">index</span> <span class="o">=</span> <span class="n">item</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span>
      <span class="n">data</span><span class="p">.</span><span class="nf">select</span> <span class="k">do</span> <span class="o">|</span><span class="n">e</span><span class="o">|</span>
        <span class="k">if</span> <span class="n">e</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span><span class="o">==</span><span class="n">index</span><span class="o">+</span><span class="mi">1</span> <span class="k">then</span> <span class="n">prior</span> <span class="o">=</span> <span class="n">e</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="k">end</span>
        <span class="k">if</span> <span class="n">e</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span><span class="o">==</span><span class="n">index</span><span class="o">-</span><span class="mi">1</span> <span class="k">then</span> <span class="n">following</span> <span class="o">=</span> <span class="n">e</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="k">end</span>
      <span class="k">end</span>
      <span class="n">set</span> <span class="o">=</span> <span class="p">[</span><span class="n">item</span><span class="p">[</span><span class="mi">0</span><span class="p">],</span> <span class="n">prior</span><span class="p">,</span> <span class="n">following</span><span class="p">]</span>
      <span class="k">return</span> <span class="n">set</span>
    <span class="k">end</span>

    <span class="k">def</span> <span class="nf">oddjob</span> <span class="n">title</span><span class="p">,</span> <span class="n">percentages</span>
      <span class="n">filename</span> <span class="o">=</span> <span class="s2">"</span><span class="si">#{</span><span class="n">title</span><span class="si">}</span><span class="s2">_</span><span class="si">#{</span><span class="no">Date</span><span class="p">.</span><span class="nf">today</span><span class="si">}</span><span class="s2">.json"</span>
      <span class="n">jf</span> <span class="o">=</span> <span class="nb">open</span><span class="p">(</span> <span class="n">filename</span><span class="p">,</span> <span class="s1">'w+'</span><span class="p">)</span>
      <span class="n">jf</span><span class="p">.</span><span class="nf">write</span><span class="p">(</span> <span class="n">percentages</span><span class="p">.</span><span class="nf">to_json</span> <span class="p">)</span>
      <span class="n">jf</span><span class="p">.</span><span class="nf">close</span>
      <span class="nb">p</span> <span class="s2">"Backup saved to </span><span class="si">#{</span><span class="n">filename</span><span class="si">}</span><span class="s2">."</span>
      <span class="c1"># save to db ? firebase?</span>
    <span class="k">end</span>

  <span class="k">end</span><span class="c1">#finex</span>

  <span class="k">class</span> <span class="nc">Helper</span>
    <span class="k">def</span> <span class="nf">make_index</span> <span class="n">pagename</span><span class="p">,</span> <span class="n">jsonlink</span><span class="p">,</span> <span class="n">title</span><span class="p">,</span> <span class="n">col1</span><span class="p">,</span> <span class="n">col2</span>
      <span class="n">content</span> <span class="o">=</span> <span class="s2">"&lt;script src=</span><span class="se">\"</span><span class="s2">https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js</span><span class="se">\"</span><span class="s2">&gt;&lt;/script&gt;
      &lt;script src=</span><span class="se">\"</span><span class="s2">https://www.gstatic.com/charts/loader.js</span><span class="se">\"</span><span class="s2">&gt;&lt;/script&gt;
      &lt;div id=</span><span class="se">\"</span><span class="si">#{</span><span class="n">pagename</span><span class="si">}</span><span class="se">\"</span><span class="s2">&gt;&lt;/div&gt;
      &lt;script&gt;
      google.charts.load('current', {packages: ['corechart', 'bar']});
      google.charts.setOnLoadCallback(drawMultSeries);
      function drawMultSeries() {
            var jsonData = $.ajax({
            url: </span><span class="se">\"</span><span class="s2">https://jsonp.afeld.me/?url=</span><span class="si">#{</span><span class="n">jsonlink</span><span class="si">}</span><span class="se">\"</span><span class="s2">,
            dataType: </span><span class="se">\"</span><span class="s2">json</span><span class="se">\"</span><span class="s2">,
            async: false
            }).responseText;
            var actual = $.parseJSON(jsonData);
            var data = new google.visualization.DataTable(jsonData);
            data.addColumn('string', 'Days of the week');
            data.addColumn('number', '</span><span class="si">#{</span><span class="n">col1</span><span class="si">}</span><span class="s2">');
            data.addColumn('number', '</span><span class="si">#{</span><span class="n">col2</span><span class="si">}</span><span class="s2">');
            data.addRows(actual[1]);
            var options = {
              title: '</span><span class="si">#{</span><span class="n">title</span><span class="si">}</span><span class="s2">',
              legend: { position: 'bottom' },
              hAxis: { title: 'Days of the week (Monday - Sunday)'},
              vAxis: { title: 'Percentage of total' }
            };
            var chart = new google.visualization.ColumnChart(document.getElementById('</span><span class="si">#{</span><span class="n">pagename</span><span class="si">}</span><span class="s2">'));
            chart.draw(data, options);
          }
      &lt;/script&gt;"</span>
      <span class="n">jf</span> <span class="o">=</span> <span class="nb">open</span><span class="p">(</span> <span class="s2">"</span><span class="si">#{</span><span class="n">pagename</span><span class="si">}</span><span class="s2">.html"</span><span class="p">,</span> <span class="s1">'w+'</span><span class="p">)</span>
      <span class="n">jf</span><span class="p">.</span><span class="nf">write</span><span class="p">(</span> <span class="n">content</span> <span class="p">)</span>
      <span class="n">jf</span><span class="p">.</span><span class="nf">close</span>
    <span class="k">end</span>

  <span class="k">end</span>
<span class="c1"># array &lt;&lt; obj</span>

<span class="k">end</span><span class="c1">#daily</span></code></pre></figure>

<p>The main ruby file that should be executed below. It pulls a free Quandl dataset which is updated daily - no need to change the json url in the Daily::Finex.new call. Unless you have a better source, that is.</p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby"><span class="nb">require_relative</span> <span class="s2">"mod_daily.rb"</span>
<span class="kp">include</span> <span class="no">Daily</span>

<span class="c1"># this is where the actual datset lives, dont change this</span>
<span class="no">Daily</span><span class="o">::</span><span class="no">Finex</span><span class="p">.</span><span class="nf">new</span><span class="p">(</span><span class="s2">"https://www.quandl.com/api/v3/datasets/BITFINEX/BTCUSD.json"</span><span class="p">).</span><span class="nf">counter_tmr</span>

<span class="c1"># pagename, jsonlink, title, col1, col2</span>
<span class="no">Daily</span><span class="o">::</span><span class="no">Helper</span><span class="p">.</span><span class="nf">new</span><span class="p">.</span><span class="nf">make_index</span><span class="p">(</span>
<span class="s2">"bitfinex"</span><span class="p">,</span>
<span class="s2">"https://changeme.com/daily/today-tmr.json"</span><span class="p">,</span>
<span class="s2">"Bitfinex BTCUSD Daily (2014-2016)"</span><span class="p">,</span>
<span class="s2">"Good To Long (%)"</span><span class="p">,</span>
<span class="s2">"Good To Short (%)"</span>
<span class="p">)</span></code></pre></figure>
