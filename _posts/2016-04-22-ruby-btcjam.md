---
layout: post
title: (Ruby) Bitcoin Trading Bots in Ruby
categories: [lending, bots]
tags: [ruby, lending, bots]
repo: https://github.com/altcointrading/btcjam-check
permalink: btcjam-check/
description: Command line tool to check open loan offers, grouping them and separating those by your selected favorite borrowers.
---


<p>Command line tool to check open loan offers, grouping them and separating those by your selected favorite borrowers.</p>

<p>Created and tested with <strong>Ruby 2.2</strong>.</p>

<figure class="highlight"><pre><code class="language-ruby" data-lang="ruby"><span class="n">gems</span>
<span class="o">----</span>

<span class="nb">require</span> <span class="s1">'httparty'</span>
<span class="nb">require</span> <span class="s1">'nokogiri'</span>
<span class="nb">require</span> <span class="s1">'json'</span>
<span class="nb">require</span> <span class="s1">'csv'</span>
<span class="nb">require</span> <span class="s1">'yaml'</span>
<span class="nb">require</span> <span class="s1">'colorize'</span>


<span class="n">config</span><span class="p">.</span><span class="nf">yaml</span>
<span class="o">-----------</span>

<span class="ss">key: </span><span class="mi">204</span><span class="n">fwe00can00start00a345rrr0i0i0i000tttt8d83724d3615</span>
<span class="ss">secret: </span><span class="mi">666</span><span class="no">Niggas0bitches0killers0bustasHustlers0ballers0gangstas0hoes35d</span>
<span class="ss">userids:
</span><span class="o">-</span> <span class="mi">14200</span>
<span class="o">-</span> <span class="mi">206241</span>
<span class="o">-</span> <span class="mi">15522</span>
<span class="o">-</span> <span class="mi">20908</span></code></pre></figure>


<br>

<img class="img-responsive" src="https://bestbitcoinexchange.co/images/btc-loans/btcjam/btcjam-id.png" alt="Configurable text script to check BTCJam users that you are interested in." />

</div>
