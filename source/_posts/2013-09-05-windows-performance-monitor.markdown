---
layout: post
title: "Windows Performance Monitor"
date: 2013-09-05 21:55
comments: true
categories: 
- Performance
---
## 總結
> I'm Watching You。

當想要監控系統的效能時，總是需要一些工具。目前使用的主要還是 Windows。針對 Windows 的資源監控工具作點紀錄。

## Windows Performance Monitor
0. 啟動 perfmon (或 perfmon.msc)
0. 點選效能監視器
0. 右鍵【新增計數器】

## typeperf 
perfmon 對應到 typeperf 的指令

我把他分兩類：有例項和沒有例項。

有例項：
typeperf "\物件(例項)\計數器"
例如：
<pre>
typeperf "\Processor Information(_Total)\% Idle Time"
</pre>

沒有例項：
typeperf "\物件\計數器"
例如：
<pre>
typeperf "\TCPv6\Connection Failures"
</pre>

### typeperf 相關參數
typeperf -h 可以看到還有一個完整路徑的寫法：
{% codeblock typeperf %}
計數器是效能計數器的完整名稱，格式為
"\\<Computer>\<Object>(<Instance>)\<Counter>"  ，
例如 "\\Server1\Processor(0)\% User Time"。
{% endcodeblock %}

用來查詢所有的計數器：
<pre>
typeperf -q
</pre> 

用來查詢該物件所有的計數器：
<pre>
typeperf -q 物件
</pre>

用來查詢該物件所有的例項與計數器組合：
<pre>
typeperf -qx 物件
</pre>

例項使用星號(*)，代表監控全部例項：
<pre>
typeperf "\Processor Information(*)\% Idle Time"
</pre>