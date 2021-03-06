---
layout:     post
title:      Hbase在HDFS上的各个目录
---
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
								            <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/ck_htmledit_views-f76675cdea.css">
						<div class="htmledit_views" id="content_views">
                
<p><span style="color:#003000;"></span></p>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">一、0.94-cdh4.2.1版本</span></span></div>
<span></span>
<div>系统级别的一级目录如下，用户自定义的均在这个/hbase 下的一级子目录下</div>
<span></span>
<div>/hbase/-ROOT-</div>
<span></span>
<div>/hbase/.META.</div>
<span></span>
<div>/hbase/.archive</div>
<span></span>
<div>/hbase/.corrupt</div>
<span></span>
<div>/hbase/.hbck</div>
<span></span>
<div>/hbase/.logs</div>
<span></span>
<div>/hbase/.oldlogs</div>
<span></span>
<div>/hbase/.snapshot</div>
<span></span>
<div>/hbase/.tmp</div>
<span></span>
<div>/hbase/hbase.id</div>
<span></span>
<div>/hbase/hbase.version</div>
<span></span>
<div> </div>
<span></span>
<div><span style="color:#000000;"><span style="font-size:13px;line-height:19.5px;margin-left:0px;">1、/hbase/-ROOT-</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">    </span></span> hbase读写数据的时候采用三级寻址方式，首先找到从 zk 中找到ROOT 表所在位置，通过 ROOT 表找到
 META 表所在位置，然后再从 META 表定位到你要读写数据Region 所在的Regionserver。所以-ROOT-目录对应 HBase 中的系统表ROOT，就不多做解释了。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">2、/hbase/.META.</span></span></div>
<span></span>
<div>     就是存储1中介绍的 META 表的存储路径。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">3、/hbase/.archive</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">     </span></span>HBase 在做 Split或者 compact 操作完成之后，会将 HFile 移到.archive 目录中，然后将之前的
 hfile 删除掉，该目录由 HMaster 上的一个定时任务定期去清理。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">4、/hbase/.corrupt</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">     </span></span>存储HBase做损坏的日志文件，一般都是为空的。<span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;"> </span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">5、/hbase/.hbck</span></span></div>
<span></span>
<div>     HBase 运维过程中偶尔会遇到元数据不一致的情况，这时候会用到提供的 hbck 工具去修复，修复过程中会使用该目录作为临时过度缓冲。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">6、/hbase/.logs</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">     </span></span>大家都知道 HBase 是支持 WAL（Write Ahead Log） 的，HBase 会在第一次启动之初会给每一台
 RegionServer 在.log 下创建一个目录，若客户端如果开启WAL 模式，会先将数据写入一份到.log 下，当 RegionServer crash 或者目录达到一定大小，会开启 replay 模式，类似 MySQL 的 binlog。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">7、/hbase/.oldlogs</span></span></div>
<span></span>
<div>当.logs 文件夹中的 HLog 没用之后会 move 到.oldlogs 中，HMaster 会定期去清理。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">8、/hbase/.snapshot</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">     </span></span>hbase若开启了 snapshot 功能之后，对某一个用户表建立一个 snapshot 之后，snapshot
 都存储在该目录下，如对表test 做了一个 名为sp_test 的snapshot，就会在/hbase/.snapshot/目录下创建一个sp_test 文件夹，snapshot 之后的所有写入都是记录在这个 snapshot 之上。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">9、/hbase/.tmp</span></span></div>
<span></span>
<div>     当对表做创建或者删除操作的时候，会将表move 到该 tmp 目录下，然后再去做处理操作。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">10、/hbase/hbase.id</span></span></div>
<span></span>
<div>     它是一个文件，存储集群唯一的 cluster id 号，是一个 uuid。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">11、/hbase/hbase.version</span></span></div>
<span></span>
<div>     同样也是一个文件，存储集群的版本号，貌似是加密的，看不到，只能通过web-ui 才能正确显示出来。</div>
<span></span>
<div> </div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">二、0.98.8版本</span></span></div>
<span></span>
<div>自0.96版本之后，hbase 源码结构上做了很大的优化，目录结构也发生了变化，做了精简和优化，这里以0.98.8为例介绍，目录如下：</div>
<span></span>
<div>/hbase/.tmp</div>
<span></span>
<div>/hbase/WALs</div>
<span></span>
<div>/hbase/archive</div>
<span></span>
<div>/hbase/corrupt</div>
<span></span>
<div>/hbase/data</div>
<span></span>
<div>/hbase/hbase.id</div>
<span></span>
<div>/hbase/hbase.version</div>
<span></span>
<div>/hbase/oldWALs</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">1、/hbase/.tmp</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">这个目录不变还是原来的tmp目录，作用是一样的。</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">2、/hbase/WALs</span></span></div>
<span></span>
<div>这里对应0.94的.logs 目录，取名为 WALs 更加见名知意了，点个赞！</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">3、/hbase/archive</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">和0.94一样，只是去掉了.而已，估计是作者不想把它作为一个隐藏文件夹了吧</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">4、/hbase/corrupt</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">和0.94一样，去了.</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">5、/hbase/data</span></span></div>
<span></span>
<div>这个才是 hbase 的核心目录，0.98版本里支持 namespace 的概念模型，系统会预置两个 namespace 即：hbase和default</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">5.1 /hbase/data/default</span></span></div>
<span></span>
<div>     这个默认的namespace即没有指定namespace 的表都将会flush 到该目录下面。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">5.2 /hbase/data/hbase</span></span></div>
<span></span>
<div>     这个namespace 下面存储了 HBase 的 namespace、meta 和acl 三个表，这里的 meta 表跟0.94版本的.META.是一样的，自0.96之后就已经将 ROOT 表去掉了，直接从Zookeeper 中找到meta 表的位置，然后通过 meta 表定位到 region。 namespace 中存储了 HBase 中的所有 namespace 信息，包括预置的hbase 和 default。acl 则是表的用户权限控制。</div>
<span></span>
<div>     如果自定义一些 namespace 的话，就会再/hbase/data 目录下新建一个 namespace 文件夹，该 namespace 下的表都将 flush 到该目录下。</div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">6、/hbase/hbase.id</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">     </span></span>它是一个文件，存储集群唯一的 cluster id 号，是一个 uuid<span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">。</span><br style="font-size:13px;line-height:19.5px;margin-left:0px;"></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">7、/hbase/hbase.version</span></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">     </span></span>同样也是一个文件，存储集群的版本号，貌似是加密的，看不到，只能通过web-ui 才能正确显示出来<span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">。</span><br style="font-size:13px;line-height:19.5px;margin-left:0px;"></span></div>
<span></span>
<div><span style="font-size:13px;line-height:19.5px;margin-left:0px;"><span style="color:#000000;">8、/hbase/oldWALs</span></span></div>
<span></span>
<div>这里对应0.94的.oldlogs 目录，取名为 oldWALs 是不是更好了呢！</div>
<p></p>
            </div>
                </div>