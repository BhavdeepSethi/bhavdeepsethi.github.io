---
layout: post
title: "Mysql Replication from an existing slave"
date: 2014-02-01 18:59:25 -0500
comments: true
categories: [tech, mysql, replication]
---

<p>So you want to add another slave to an existing mysql replication environment? </p>
<p>There are multiple ways to do this. </p>
<p><strong>First option, as mentioned in the mysql documentation: </strong></p>
<p><a href="http://dev.mysql.com/doc/refman/5.5/en/replication-howto-additionalslaves.html" title="Replication How to add additional slaves" target="_blank">Replication How to add additional slaves</a></p>
<p>This is the standard way most people follow. The issue with this is that you need to shutdown your mysql slave in this case. If it is the only slave present, it might not be feasible to do so.<br />
You may use stop slave instead of a complete shutdown, but that might not be feasible as well. Your system probably cannot afford any replication lag.<br />
Plus, the whole process is not straightforward and really prone to errors. </p>
<p><strong>Another option is to use lvm snapshot to take backups: </strong><br />
<!--more -->
<span id="more-187"></span><br />
<a href="http://www.mysqlperformanceblog.com/2006/08/21/using-lvm-for-mysql-backup-and-replication-setup/" title="Using lvm for mysql backup and replication setup" target="_blank">Using lvm for mysql backup and replication setup</a></p>
<p>I've used this multiple times and it works like a charm. It is very fast to do with almost 0 downtime/lag. Make sure you copy the data to another server using rsync in a screen. </p>
<p>But what if you want to start partial replication? Say, you want to replicate only 2-3 tables from the entire mysql instance? </p>
<p>Both the methods mentioned here copy the entire mysql data files (/var/lib/mysql). You can obviously do so for partial replication as well and delete the unnecessary tables, but it is a really inefficient way of doing this. </p>
<p>There is a better way to do this.<br /><br />
<strong>Using mysqldump (for mysql >= 5.5.3): </strong></p>
<p>Let us see the query to dump a single table from a single database:<br /><br />
<code language="sql"><br />
mysqldump --single-transaction --flush-logs --dump-slave=2 --max_allowed_packet=512M -u root -p database_name table_name | gzip > dump_file_name.gz<br />
</code> <br />
<br />

Let us go through the options:<br />
&#8211;single-transaction: This basically starts the dump process under a new transaction without blocking any other applications.</p>
<p>Read more: <a href="http://dev.mysql.com/doc/refman/5.5/en/mysqldump.html#option_mysqldump_single-transaction" title="mysqldump: single-transaction" target="_blank">mysqldump: single-transaction</a></p>
<p>&#8211;flush-logs: Flushes the MySQL server log files before starting the dump. </p>
<p>Read more: <a href="http://dev.mysql.com/doc/refman/5.5/en/mysqldump.html#option_mysqldump_flush-logs" title="mysqldump: flush-logs" target="_blank">mysqldump: flush-logs</a></p>
<p>&#8211;dump-slave: This is the heart of the query. It is used to print the binary log coordinates (file name and position) of the dumped slave's master in the dump file.<br />
&#8211;dump-slave=1 prints the actual change master to statement in the sql dump.<br />
Changing the value to 2 causes it to print the same but within a comment. Essentially, you can run it at your own convenience.<br />
If you are replicating from a slave use &#8211;dump-slave. If you are running this query on the master itself, replace this with &#8211;master-data.<br />
Read more: <a href="http://dev.mysql.com/doc/refman/5.5/en/mysqldump.html#option_mysqldump_dump-slave " title="mysqldump: dump-slave" target="_blank">mysqldump: dump-slave</a></p>
<p>&#8211;max_allowed_packet: The maximum size of the buffer for client/server communication. The default is 24MB, the maximum is 1GB </p>
<p>gzip: Compress the data while being writhed into a dump file. You will see a good 70%~80% compression rate. This helps a lot when transferring files cross DC.</p>
<p>To run this for multiple tables use the &#8211;tables option. If you want to run it for one or more databases, use &#8211;databases option.<br />
You can use this for full replication as well as long as you're using InnoDB tables. </p>
<p>Basically you can use any of the <a href="http://dev.mysql.com/doc/refman/5.5/en/mysqldump.html" title="mysqldump" target="_blank">mysqldump</a> flags as per your requirement. </p>