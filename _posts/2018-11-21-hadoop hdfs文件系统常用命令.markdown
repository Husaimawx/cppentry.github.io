---
layout:     post
title:      hadoop hdfs文件系统常用命令
---
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
								            <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/ck_htmledit_views-f76675cdea.css">
						<div class="htmledit_views" id="content_views">
                
<div id="article_content" class="article_content">
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop常用命令： </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
查看Hadoop HDFS支持的所有命令 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –ls </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
列出目录及文件信息 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –lsr </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
循环列出目录、子目录及文件信息 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –put test.txt /user/sunlightcs </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
将本地文件<a href="http://www.2cto.com/os/" rel="nofollow" class="keylink" style="color:rgb(51,51,51);text-decoration:none;">系统</a>的test.txt复制到HDFS文件系统的/user/sunlightcs目录下 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –get /user/sunlightcs/test.txt . </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
将HDFS中的test.txt复制到本地文件系统中，与-put命令相反 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –cat /user/sunlightcs/test.txt </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
查看HDFS文件系统里test.txt的内容 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –tail /user/sunlightcs/test.txt </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
查看最后1KB的内容 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –rm /user/sunlightcs/test.txt </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
从HDFS文件系统删除test.txt文件，rm命令也可以删除空目录 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –rmr /user/sunlightcs </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
删除/user/sunlightcs目录以及所有子目录 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –copyFromLocal test.txt /user/sunlightcs/test.txt </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
从本地文件系统复制文件到HDFS文件系统，等同于put命令 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –copyToLocal /user/sunlightcs/test.txt test.txt </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
从HDFS文件系统复制文件到本地文件系统，等同于get命令 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –chgrp [-R] /user/sunlightcs </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
修改HDFS系统中/user/sunlightcs目录所属群组，选项-R递归执行，跟linux命令一样 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –chown [-R] /user/sunlightcs </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
修改HDFS系统中/user/sunlightcs目录拥有者，选项-R递归执行 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –chmod [-R] MODE /user/sunlightcs </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
修改HDFS系统中/user/sunlightcs目录权限，MODE可以为相应权限的3位数或+/-{rwx}，选项-R递归执行 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –count [-q] PATH </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
查看PATH目录下，子目录数、文件数、文件大小、文件名/目录名 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –cp SRC [SRC …] DST      </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
将文件从SRC复制到DST，如果指定了多个SRC，则DST必须为一个目录 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –du PATH </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
显示该目录中每个文件或目录的大小 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –dus PATH </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
类似于du，PATH为目录时，会显示该目录的总大小 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –expunge </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
清空回收站，文件被删除时，它首先会移到临时目录.Trash/中，当超过延迟时间之后，文件才会被永久删除 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –getmerge SRC [SRC …] LOCALDST [addnl]     </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
获取由SRC指定的所有文件，将它们合并为单个文件，并写入本地文件系统中的LOCALDST，选项addnl将在每个文件的末尾处加上一个换行符 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –touchz PATH  </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
创建长度为0的空文件 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –test –[ezd] PATH    </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
对PATH进行如下类型的检查： </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
-e PATH是否存在，如果PATH存在，返回0，否则返回1 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
-z 文件是否为空，如果长度为0，返回0，否则返回1 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
-d 是否为目录，如果PATH为目录，返回0，否则返回1 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –text PATH </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
显示文件的内容，当文件为文本文件时，等同于cat，文件为压缩格式（gzip以及hadoop的二进制序列文件格式）时，会先解压缩 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
 </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
hadoop fs –help ls </div>
<div style="border:0px;list-style:none;color:rgb(51,51,51);font-family:'宋体';font-size:14px;line-height:28px;">
查看某个[ls]命令的帮助文档</div>
</div>
            </div>
                </div>