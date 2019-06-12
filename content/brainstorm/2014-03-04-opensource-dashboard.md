---
author: brainstorm
date: '2014-03-03T13:00:00.000000+00:00'
excerpt: null
layout: post
modified: '2014-03-03T13:00:00.000000+00:00'
permalink: https://blogs.nopcode.org/brainstorm/2014/03/04/opensource-dashboard/
tags:
- data science
- elasticsearch
- kibana
- logstash
- programming
- sysadmin
title: 'My take on the ELK stack: best opensource dashboard'
---

A dashboard with pretty plots and numbers about your organization... **It shouldn't be that difficult, right**?

<blockquote class="twitter-tweet" width="550" lang="en">
  <p>
    Reality: Things don’t always go according to the plan&#10;<a href="https://t.co/tD5W5Eqcbd">pic.twitter.com/tD5W5Eqcbd</a> via <a href="https://twitter.com/ToddWhitaker">@ToddWhitaker</a> & <a href="https://twitter.com/LarysaDiDio">@LarysaDiDio</a>
  </p>&mdash; Michael Carton (@michaeltcarton) 
  
  <a href="https://twitter.com/michaeltcarton/statuses/440090667872575488">March 2, 2014</a>
</blockquote>

Luckily, ElasticSearch+Logstash+Kibana did came up with a nice stack to solve this problem. Here's my result and **how I solved some integration hiccups**. The resulting beta dashboard, as it is now, looks like this:

[<img src="https://blogs.nopcode.org/brainstorm/wp-content/uploads/2014/03/dataspace_blurred-300x163.png" alt="DataSpace beta operations panel" width="300" height="163" class="aligncenter size-large wp-image-842" />][1]

[Many][2] [many][3] [many][4] [other][5] [blog][6] [posts][7] have addressed this issue, but I would like to share a couple of tweaks I came up with while working on it.

<!--more-->

## Logstash and iRODs custom log parsing

Broadly speaking, I found the following issues when setting up the dashboard:

1.  [Index design:][8] Logstash default index formatting (logstash-YYYYMMDD) [**brought down a 30GB RAM machine**][9] (line 63).
2.  **Missing year** in log timestamps from logs content, **getting it from log filenames** (lines 31 to 42 below).
3.  [GeoIP coordinates][10] not being parsed by Kibana's bettermap (lines 44 to 58).

<noscript>
  <p>
    View the code on <a href="https://gist.github.com/6552989">Gist</a>.
  </p>
</noscript>

## ElasticSearch tweaks

As mentioned, the default **"daily" indexing scheme in logstash did not work** for my purposes. When monitoring elasticsearch with the [head plugin][11], the status went red while ingesting events from logs. Thanks to [@zackarytong][12] and other's feedback, I managed to address the issue:

<blockquote class="twitter-tweet" width="550" lang="en">
  <p>
    Stress test <a href="https://twitter.com/search?q=%23elasticsearch&src=hash">#elasticsearch</a>+<a href="https://twitter.com/search?q=%23logstash&src=hash">#logstash</a> with 3 years (800MB) worth of logs to parse and index. If the machine survives, we'll call it a day :)
  </p>&mdash; Roman Valls (@braincode) 
  
  <a href="https://twitter.com/braincode/statuses/432918960900964352">February 10, 2014</a>
</blockquote>

Then, **Kibana could not connect to the elasticsearch** backend when large time ranges were defined. After some chrome developer tool and CURLing, I reported [the issue][13], **too many indexes were URL-encoded** which required to setup the following ElasticSearch directive:

<pre>http.max_initial_line_length: 64kb</pre>

In the future, I might consider **further indexing optimizations** by using the [ElasticSearch index curator tool][14] to optimize and cut down index usage, but for now I want to keep all data accessible from the dashboard.

## Kibana

The presentation side of this **dashboard hipster stack also had some gimmicks**. My original idea of showing three separate Kibana bettermaps, one per AWS region, had to wait after [another issue got addressed very recently][15]. Both Chrome developer console and [AngularJS batarang][16] were very useful to find issues in Kibana and its interactions with the ElasticSearch backend.

Speaking of Amazon and their regions, while AWS has exposed a great amount of functionality through their developer APIs, **at the time of writing these lines there are missing API endpoints**, such as billing information in all regions (only available in **us-east-1**), and **fetching remaining credits** from your [EDU Amazon grant][17], should you have one. **The only viable option left today is scraping it**:

<noscript>
  <p>
    View the code on <a href="https://gist.github.com/9095793">Gist</a>.
  </p>
</noscript>

If one wants to keep track of AWS expenses on ES, some specific [index type mappings][18] changes on the ElasticSearch side are needed:

<pre class="brush: bash; title: ; notranslate" title="">curl -XPUT 'https://ids-panel.incf.net:9200/aws-2014.02/credits/_mapping' -d '{
  "aws_credit_balance": {
    "properties": {
      "aws_credit_balance": {
        "type": "integer"
      }
    }
  }
}'
</pre>

In a more large scale/business setting, [Netflix's Ice][19] could be definitely a good fit.

## Conclusions and future improvements

It has been a fun project to **collect data about your organization's services and be able to expose it as clearly as possible and in realtime**, feels like I would like to do this for a living as a consultant some day. Some **new insight coming from the dashboard** has allowed us to decide on downscale resources, **ultimately saving money**.

**The feedback from INCF people has been invaluable to rethink how some data is presented** and what it all means, always bring third parties and ask them their opinions. **Visualization is hard to get right, bring in users and consider their feedback**.

My next iteration in this project is to **have finer detail on which activities users are performing** (data being shared, copied, downloaded). This could be leveraged with some custom [iRODS microservices for ElasticSearch][20] or evaluating other EU-funded projects in the topic such as [Chesire3][21].

When it comes to who can access the dashboard, there's a recent [blog post on multi-faceted authentication][22], a.k.a **showing different views of the dashboard to different audiences**. I've already tried [Kibana's authentication proxy][23], which supports OAuth among other auth systems, but there are a few rough edges to polish.

On the logstash backend, **it might be worth grepping iRODS codebase for log stanzas** to assess **important log events worth parsing** and getting good semantic tokens out of them. Luckily, ElasticSearch is backed by [Lucene's][24] **full text engine helps a lot** in not having to do this tedious task. **Kibana/ElasticSearch search and filtering are excellent**.

Last but not least, some remaining issues leading to total world domination include:

1.  Instrumenting all your organization's Python code with [Logbook sinking to a Redis exchange.][25]
2.  **Easily add/include other types of panels in Kibana**, perhaps allowing better or more explicit integration possibilities for D3, [mpld3][26] or [BokehJS][27] with Kibana.
3.  [Getting UNIQ count for records in ElasticSearch][28] (i.e, count unique number of IPs, users, etc...) which are on the roadmap under [aggregations][29], so they are coming soon :)

 [1]: https://blogs.nopcode.org/brainstorm/wp-content/uploads/2014/03/dataspace_blurred.png
 [2]: https://blog.trifork.com/2014/01/28/using-logstash-elasticsearch-and-kibana-to-monitor-your-video-card-a-tutorial/
 [3]: https://www.jaddog.org/2014/01/16/openstack-logstash-elasticsearch-kibana/
 [4]: https://nhhagen.wordpress.com/2013/11/28/query-log-analysis-using-logstash-elasticsearch-and-kibana/
 [5]: https://michael.bouvy.net/blog/en/2013/11/19/collect-visualize-your-logs-logstash-elasticsearch-redis-kibana/
 [6]: https://krisjordan.com/essays/log-warehousing-with-logstash-and-kibana
 [7]: https://speakerdeck.com/elasticsearch/using-elasticsearch-logstash-and-kibana-to-create-realtime-dashboards
 [8]: https://www.elasticsearch.org/blog/using-elasticsearch-and-logstash-to-serve-billions-of-searchable-events-for-customers/
 [9]: https://twitter.com/braincode/status/432918960900964352
 [10]: https://bryanw.tk/2013/geoip-in-logstash-kibana/
 [11]: https://mobz.github.io/elasticsearch-head/
 [12]: https://twitter.com/ZacharyTong
 [13]: https://github.com/elasticsearch/kibana/issues/962
 [14]: https://www.elasticsearch.org/blog/curator-tending-your-time-series-indices/
 [15]: https://github.com/elasticsearch/kibana/issues/952
 [16]: https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk
 [17]: https://aws.amazon.com/education/aws-in-education-research-grants/
 [18]: https://www.elasticsearch.org/blog/changing-mapping-with-zero-downtime/
 [19]: https://github.com/Netflix/ice
 [20]: https://groups.google.com/forum/#!topic/irod-chat/XpezH-N2B4Y
 [21]: https://cheshire3.org/
 [22]: https://www.elasticsearch.org/blog/restricting-users-kibana-filtered-aliases/
 [23]: https://github.com/fangli/kibana-authentication-proxy
 [24]: https://lucene.apache.org/core/
 [25]: https://mussolblog.wordpress.com/2013/10/02/determining-buffer-size-for-redishandler-in-python-logbook/
 [26]: https://github.com/jakevdp/mpld3
 [27]: https://continuumio.github.io/bokehjs/
 [28]: https://twitter.com/zacharytong/status/434368851988709376
 [29]: https://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-aggregations.html