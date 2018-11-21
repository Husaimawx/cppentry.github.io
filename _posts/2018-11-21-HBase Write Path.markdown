---
layout:     post
title:      HBase Write Path
---
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
								            <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/ck_htmledit_views-f76675cdea.css">
						<div class="htmledit_views" id="content_views">
                
<p style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Apache <span style="font-style:inherit;color:rgb(47,43,44);font-size:13px;">HBase</span> is the Hadoop database, and is based on the Hadoop Distributed File System (<span style="font-style:inherit;color:rgb(47,43,44);font-size:13px;">HDFS</span>). HBase makes
 it possible to randomly access and update data stored in HDFS, but files in HDFS can only be appended to and are immutable after they are created.  So you may ask, how does HBase provide low-latency reads and writes? In this blog post, we explain this by describing
 the write path of HBase — how data is updated in HBase.</p>
<p style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
The <span style="font-style:inherit;color:rgb(47,43,44);font-size:13px;">write path</span> is how an HBase completes put or delete operations. This path begins at a client, moves to a region server, and ends when data eventually is written to an HBase data
 file called an <span style="font-style:inherit;color:rgb(47,43,44);font-size:13px;">HFile</span>. Included in the design of the write path are features that HBase uses to prevent data loss in the event of a region server failure. Therefore understanding the
 write path can provide insight into HBase’s native data loss prevention mechanism.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Each HBase table is hosted and managed by sets of servers which fall into three categories:</p>
<ol class="c9" start="1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);"><li class="c0" style="list-style:decimal;">
One active master server</li><li class="c0" style="list-style:decimal;">
One or more backup master servers</li><li class="c0" style="list-style:decimal;">
Many region servers</li></ol><p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Region servers contribute to handling the HBase tables. Because HBase tables can be large, they are broken up into partitions called regions. Each region server handles one or more of these regions. Note that because region servers are the only servers that
 serve HBase table data, a master server crash cannot cause data loss.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
HBase data is organized similarly to a sorted map, with the sorted key space partitioned into different shards or regions. An HBase client updates a table by invoking put or delete commands. When a client requests a change, that request is routed to a region
 server right away by default. However, programmatically, a client can cache the changes in the client side, and flush these changes to region servers in a batch, by turning the autoflush off. If autoflush is turned off, the changes are cached until flush-commits
 is invoked, or the buffer is full depending on the buffer size set programmatically or configured with parameter “hbase.client.write.buffer”.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Since the row key is sorted, it is easy to determine which region server manages which key. A change request is for a specific row. Each row key belongs to a specific region which is served by a region server. So based on the put or delete’s key, an HBase client
 can locate a proper region server. At first, it locates the address of the region server hosting the -ROOT- region from the ZooKeeper quorum.  From the root region server, the client finds out the location of the region server hosting the -META- region.  From
 the meta region server, then we finally locate the actual region server which serves the requested region.  This is a three-step process, so the region location is cached to avoid this expensive series of operations. If the cached location is invalid (for
 example, we get some unknown region exception), it’s time to re-locate the region and update the cache.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
After the request is received by the right region server, the change cannot be written to a HFile immediately because the data in a HFile must be sorted by the row key. This allows searching for random rows efficiently when reading the data. Data cannot be
 randomly inserted into the HFile. Instead, the change must be written to a new file. If each update were written to a file, many small files would be created. Such a solution would not be scalable nor efficient to merge or read at a later time. Therefore,
 changes are not immediately written to a new HFile.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Instead, each change is stored in a place in memory called the <span style="font-style:inherit;color:rgb(47,43,44);font-size:13px;">memstore</span>, which cheaply and efficiently supports random writes. Data in the memstore is sorted in the same manner as
 data in a HFile. When the memstore accumulates enough data, the entire sorted set is written to a new HFile in HDFS. Completing one large write task is efficient and takes advantage to HDFS’ strengths.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
<a href="http://www.cloudera.com/wp-content/uploads/2012/05/write_path.png" rel="nofollow" style="text-decoration:none;color:rgb(1,126,197);"><img class="aligncenter size-large wp-image-15282" title="write_path" src="http://files.cloudera.com/images/Blog_Images/write_path.png" alt="Write Path" width="1024" height="466" style="border:0px none;"></a></p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Although writing data to the memstore is efficient, it also introduces an element of risk: Information stored in memstore is stored in volatile memory, so if the system fails, all memstore information is lost. To help mitigate this risk, HBase saves updates
 in a write-ahead-log (<span style="font-style:inherit;color:rgb(47,43,44);font-size:13px;">WAL</span>) before writing the information to memstore. In this way, if a region server fails, information that was stored in that server’s memstore can be recovered
 from its WAL.</p>
<p class="c2 c1 c6" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Note: By default, WAL is enabled, but the process of writing the WAL file to disk does consume some resources. WAL may be disabled, but this should only be done if the risk of data loss is not a concern. If you choose to disable WAL, consider implementing your
 own disaster recovery solution or be prepared for the possibility of data loss.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
The data in a WAL file is organized differently from HFile. WAL files contain a list of edits, with one edit representing a single put or delete. The edit includes information about the change and the region to which the change applies. Edits are written chronologically,
 so, for persistence, additions are appended to the end of the WAL file that is stored on disk. Because WAL files are ordered chronologically, there is never a need to write to a random place within the file.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
As WALs grow, they are eventually closed and a new, active WAL file is created to accept additional edits. This is called “rolling” the WAL file. Once a WAL file is rolled, no additional changes are made to the old file.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
By default, WAL file is rolled when its size is about 95% of the HDFS block size. You can configure the multiplier using parameter: “hbase.regionserver.logroll.multiplier”, and the block size using parameter: “hbase.regionserver.hlog.blocksize”. WAL file is
 also rolled periodically based on configured interval “hbase.regionserver.logroll.period”, an hour by default, even the WAL file size is smaller than the configured limit.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Constraining WAL file size facilitates efficient file replay if a recovery is required. This is especially important during replay of a region’s WAL file because while a file is being replayed, the corresponding region is not available. The intent is to eventually
 write all changes from each WAL file to disk and persist that content in an HFile. After this is done, the WAL file can be archived and it is eventually deleted by the LogCleaner daemon thread.  Note that WAL files serve as a protective measure. WAL files
 need only be replayed to recover updates that would otherwise be lost after a region server crash.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
A region server serves many regions, but does not have a WAL file for each region. Instead, one active WAL file is shared among all regions served by the region server. Because WAL files are rolled periodically, one region server may have many WAL files. Note
 that there is only one active WAL per region server at a given time.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Assuming the default HBase root of “/hbase”, all the WAL files for a region server instance are stored under the same root folder, which is as follows:</p>
<pre class="code" style="background-color:rgb(206,233,255);color:rgb(48,48,48);font-size:13px;line-height:20px;font-family:Monaco, 'Courier New', Courier, monospace;">/hbase/.logs/&lt;host&gt;,
&lt;port&gt;,&lt;startcode&gt;</pre>
<p style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
For example:</p>
<pre class="code" style="background-color:rgb(206,233,255);color:rgb(48,48,48);font-size:13px;line-height:20px;font-family:Monaco, 'Courier New', Courier, monospace;">/hbase/.logs/srv.example.com,60020,1254173957298</pre>
<p style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
WAL log files are named as follows:</p>
<pre class="code" style="background-color:rgb(206,233,255);color:rgb(48,48,48);font-size:13px;line-height:20px;font-family:Monaco, 'Courier New', Courier, monospace;">/hbase/.logs/&lt;host&gt;,
&lt;port&gt;,&lt;startcode&gt;/&lt;host&gt;%2C
&lt;port&gt;%2C&lt;startcode&gt;.&lt;timestamp&gt;</pre>
<p style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
For example:</p>
<pre class="code" style="background-color:rgb(206,233,255);color:rgb(48,48,48);font-size:13px;line-height:20px;font-family:Monaco, 'Courier New', Courier, monospace;">/hbase/.logs/srv.example.com,60020,1254173957298/srv.example.com%2C60020%2C1254173957298.1254173957495</pre>
<p style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Each edit in the WAL file has a unique sequence id. This id increases to preserve the order of edits. Whenever a log file is rolled, the next sequence id and the old file name are put in an in-memory map. This information is used to track the maximum sequence
 id of each WAL file so that we can easily figure out if a file can be archived at a later time when some memstore is flushed to disk.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Edits and their sequence ids are unique within a region. Any time an edit is added to the WAL log, the edit’s sequence id is also recorded as the last sequence id written. When the memstore is flushed to disk, the last sequence id written for this region is
 cleared. If the last sequence id written to disk is the same as the maximum sequence id of a WAL file, it can be concluded that all edits in a WAL file for this region have been written to disk. If all edits for all regions in a WAL file have been written
 to disk, it is clear that no splitting or replaying will be required, and the WAL file can be archived.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
WAL file rolling and memstore flush are two separate actions, and don’t have to happen together. However, we don’t want to keep too many WAL files per region server so as to avoid time-consuming recovery in case a region server failure. Therefore, when a WAL
 file is rolled, HBase checks if there are too many WAL files, and decide what regions should be flushed so that some WAL files can be archived.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
In this post, we explain the HBase write path, which is how data in HBase is created and/or updated. Some important parts of it are:</p>
<ol class="c9" start="1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);"><li class="c0" style="list-style:decimal;">
How a client locates a region server,</li><li class="c0" style="list-style:decimal;">
Memstore which supports fast random writes,</li><li class="c0" style="list-style:decimal;">
WAL files as the way to avoid data loss in case region server failures.</li></ol><p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
We will talk about HFile formats, WAL file splitting and so on in subsequent posts.</p>
<p class="c1" style="color:rgb(80,80,80);font-size:14px;line-height:20px;background-color:rgb(250,250,250);">
Ref: <a href="http://www.cloudera.com/blog/2012/06/hbase-write-path/" rel="nofollow">http://www.cloudera.com/blog/2012/06/hbase-write-path/</a></p>
            </div>
                </div>