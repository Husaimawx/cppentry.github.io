---
layout:     post
title:      Hive Shell 基本操作——深入浅出学Hive
---
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
								            <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/ck_htmledit_views-f76675cdea.css">
						<div class="htmledit_views" id="content_views">
                
<div style="border-width:0px;overflow:hidden;font-family:verdana, arial, helvetica, sans-serif;line-height:21px;background-color:rgb(250,250,250);">
<p style="font-size:14px;line-height:1.5em;">
<span class="bold" style="font-weight:bold;line-height:1.5em;">目录：</span></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/6220.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">初始Hive</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/6221.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive安装与配置</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/6222.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive 内建操作符与函数开发</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/6223.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive JDBC</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/6225.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">hive参数</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/6226.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive 高级编程</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/0/6227.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive QL</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/6228.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive Shell 基本操作</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/0/6229.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">hive 优化</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/0/6231.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive体系结构</a></p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/forum/blogPost/list/0/6232.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">Hive的原理</a></p>
<p style="font-size:14px;line-height:1.5em;">
 </p>
<p style="font-size:14px;line-height:1.5em;">
<a href="http://sishuok.com/product/561" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;line-height:1.5em;">配套视频课程</a></p>
<p style="font-size:14px;line-height:1.5em;">
 </p>
<span class="bold" style="font-size:16px;font-weight:bold;">第一部分：Hive bin下脚本介绍</span></div>
<div style="border-width:0px;overflow:hidden;font-family:verdana, arial, helvetica, sans-serif;line-height:21px;background-color:rgb(250,250,250);">
<div class="O" style="border-width:0px;overflow:hidden;">
<span class="bold" style="font-weight:bold;"><span lang="en-us" xml:lang="en-us">Hive bin</span>下的脚本介绍</span></div>
</div>
<div style="border-width:0px;overflow:hidden;font-family:verdana, arial, helvetica, sans-serif;line-height:21px;background-color:rgb(250,250,250);">
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;">Ext</div>
<div style="border-width:0px;overflow:hidden;"><img src="http://sishuok.com/forum/upload/2012/10/22/92619e8601ec23fd60b91581c642406a__1.JPG" alt="" style="border-width:0px;border-style:none;"></div>
<div style="border-width:0px;overflow:hidden;">hive</div>
<div style="border-width:0px;overflow:hidden;">hive-config</div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;"><span class="bold" style="font-size:16px;font-weight:bold;">第二部分：Hive Shell 基本操作</span></div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<span class="bold" lang="en-us" style="font-weight:bold;" xml:lang="en-us">Hive 命令行</span></div>
</div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;">•hive [-hiveconf x=y]* [&lt;-i filename&gt;]* [&lt;-f filename&gt;|&lt;-e query-string&gt;] [-S]</div>
<div style="border-width:0px;overflow:hidden;">•-i   从文件初始化HQL</div>
<div style="border-width:0px;overflow:hidden;">•-e   从命令行执行指定的HQL</div>
<div style="border-width:0px;overflow:hidden;">• -f   执行HQL脚本</div>
<div style="border-width:0px;overflow:hidden;">•-v   输出执行的HQL语句到控制台</div>
<div style="border-width:0px;overflow:hidden;">•-p &lt;port&gt; connect to Hive Server on port number -hiveconf x=y Use this to set hive/hadoop configuration variables.</div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<span class="bold" lang="en-us" style="font-weight:bold;" xml:lang="en-us">Hive 命令行示例</span></div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
•从命令行执行指定的sql语句</div>
<div class="O1" style="border-width:0px;overflow:hidden;">
•$HIVE_HOME/bin/hive -e 'select a.col from tab1 a'</div>
<div class="O" style="border-width:0px;overflow:hidden;">
•以指定的hive环境变量执行指定的sql语句</div>
<div class="O1" style="border-width:0px;overflow:hidden;">
•$HIVE_HOME/bin/hive -e 'select a.col from tab1 a' -hiveconf hive.exec.scratchdir=/home/my/hive_scratch -hiveconf mapred.reduce.tasks=32</div>
<div class="O" style="border-width:0px;overflow:hidden;">
•以沉默模式执行指定的sql语句，并将执行结果导出到指定文件 HIVE_HOME/bin/hive  -e 'select a.col from tab1 a' &gt; a.txt</div>
<div class="O" style="border-width:0px;overflow:hidden;">
•以非交互式模式执行sql文件</div>
<div class="O1" style="border-width:0px;overflow:hidden;">
•HIVE_HOME/bin/hive -f /home/my/hive-script.sql</div>
<div class="O" style="border-width:0px;overflow:hidden;">
•在进入交互模式之前，执行初始化sql文件</div>
<div class="O1" style="border-width:0px;overflow:hidden;">
•HIVE_HOME/bin/hive -i /home/my/hive-init.sql</div>
<div class="O" style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<span class="bold" style="font-weight:bold;"><span lang="en-us" xml:lang="en-us">Hive </span>交互式<span lang="en-us" xml:lang="en-us">Shell</span>命令</span></div>
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;">• 当命令 $HIVE_HOME/bin/hive以不带 -e/-f 选项的方式运行时， hive将进入到交互模式</div>
<div style="border-width:0px;overflow:hidden;">•以（;）冒号结束命令行</div>
</div>
<div style="border-width:0px;overflow:hidden;">
<table dir="ltr" cellspacing="0" cellpadding="0" style="font-size:12px;border:none;width:455px;"><tbody><tr><td bgcolor="#00CC99" width="228" height="23" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;"><span>*Command *</span></div>
</td>
<td bgcolor="#00CC99" width="228" height="23" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;"><span>Description</span></div>
</td>
</tr><tr><td bgcolor="#CBECDE" width="228" height="39" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">quit</div>
</td>
<td bgcolor="#CBECDE" width="228" height="39" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">退出命令行</div>
</td>
</tr><tr><td bgcolor="#E7F6EF" width="228" height="104" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">set &lt;key&gt;=&lt;value&gt;</div>
</td>
<td bgcolor="#E7F6EF" width="228" height="104" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">设置参数</div>
</td>
</tr><tr><td bgcolor="#CBECDE" width="228" height="55" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">set -v</div>
</td>
<td bgcolor="#CBECDE" width="228" height="55" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">打印出所有Hive支持的命令</div>
</td>
</tr></tbody></table></div>
<div style="border-width:0px;overflow:hidden;">
<table dir="ltr" cellspacing="0" cellpadding="0" style="font-size:12px;border:none;width:446px;"><tbody><tr><td bgcolor="#00CC99" width="223" height="24" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;"><span>*Command *</span></div>
</td>
<td bgcolor="#00CC99" width="223" height="24" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;"><span>Description</span></div>
</td>
</tr><tr><td bgcolor="#CBECDE" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">add FILE &lt;value&gt; &lt;value&gt;*</div>
</td>
<td bgcolor="#CBECDE" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">增加一个文件到资源列表.</div>
</td>
</tr><tr><td bgcolor="#E7F6EF" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">list FILE</div>
</td>
<td bgcolor="#E7F6EF" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">列出所有已经添加的资源</div>
</td>
</tr><tr><td bgcolor="#CBECDE" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">list FILE &lt;value&gt;*</div>
</td>
<td bgcolor="#CBECDE" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">根据value来查看添加的资源</div>
</td>
</tr><tr><td bgcolor="#E7F6EF" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">! &lt;cmd&gt;</div>
</td>
<td bgcolor="#E7F6EF" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">execute a shell command from hive shell</div>
</td>
</tr><tr><td bgcolor="#CBECDE" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">dfs &lt;dfs command&gt;</div>
</td>
<td bgcolor="#CBECDE" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">执行DFS的命令</div>
</td>
</tr><tr><td bgcolor="#E7F6EF" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">&lt;query string&gt;</div>
</td>
<td bgcolor="#E7F6EF" width="223" height="40" style="font-family:verdana, arial, helvetica, sans-serif;">
<div style="border-width:0px;overflow:hidden;">执行查询并输出到标准输出</div>
</td>
</tr></tbody></table></div>
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;"><span class="bold" style="font-size:16px;font-weight:bold;">第三部分：日志</span></div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<span class="bold" style="font-weight:bold;">日志</span></div>
</div>
<div style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
•Hive使用Log4J来处理日志</div>
<div class="O" style="border-width:0px;overflow:hidden;">
•我们可以通过下面的命令设计Hive的日志级别</div>
<div class="O1" style="border-width:0px;overflow:hidden;">
•$HIVE_HOME/bin/hive -hiveconf hive.root.logger=INFO,console</div>
<div class="O" style="border-width:0px;overflow:hidden;">
•hive.root.logger的有INFO,DEBUG, 等</div>
<div class="O1" style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;"><span class="bold" style="font-size:16px;font-weight:bold;">第四部分：资源</span></div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<span class="bold" style="font-weight:bold;"><span lang="en-us" xml:lang="en-us">Hive</span>添加资源</span></div>
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;">•Hive可以动态的添加资源，如文件</div>
<div style="border-width:0px;overflow:hidden;">•一般情况下，我们是在与Hive进行交互时添加文件</div>
<div style="border-width:0px;overflow:hidden;">•实际上是使用Hadoop的 Distributed Cache来控制的</div>
<div style="border-width:0px;overflow:hidden;">
<div class="O" style="border-width:0px;overflow:hidden;">
<span class="bold" style="font-weight:bold;">例子</span></div>
<div class="O" style="border-width:0px;overflow:hidden;">
<div style="border-width:0px;overflow:hidden;">•ADD { FILE[S] | JAR[S] | ARCHIVE[S] } &lt;filepath1&gt; [&lt;filepath2&gt;]*</div>
<div style="border-width:0px;overflow:hidden;">•LIST { FILE[S] | JAR[S] | ARCHIVE[S] } [&lt;filepath1&gt; &lt;filepath2&gt; ..]</div>
<div style="border-width:0px;overflow:hidden;">• DELETE { FILE[S] | JAR[S] | ARCHIVE[S] } [&lt;filepath1&gt; &lt;filepath2&gt; ..]</div>
<div style="border-width:0px;overflow:hidden;"> </div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
转自：<a href="http://sishuok.com/forum/blogPost/list/0/6228.html" rel="nofollow" style="color:rgb(0,94,167);text-decoration:none;">http://sishuok.com/forum/blogPost/list/0/6228.html</a></div>
</div>
            </div>
                </div>