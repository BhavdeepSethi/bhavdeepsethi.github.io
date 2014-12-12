---
layout: post
title: MySQL Partial Replication
date: 2014-02-08 18:36:09 -0500
date_formatted: February 8, 2014
comments: true
categories: [tech,mysql,replication]
---

<p>A couple of folks have asked me to elaborate more on partial replication.<br />
What is the use of replicating only certain tables? Can you add/remove tables/db to the existing replication? </p>
<p>Here is a follow up post to answer these and other such questions. </p>
<p>-What is Partial Replication?<br />
Partial replication is when your slave is replicating only certain db or tables from the master. </p>
<p>-What is the use of partial replication? </p><br />
<!--more-->
You might have cases where you might be storing a lot of data in your master instance, but only want to consume certain tables from the slave. If you're low on space, it makes sense replicating only the desired tables.<br />
There might be other reasons and this really depends on the application consuming the data.<br />
For instance, in Flipkart, most teams host their own instance of mysql on a different box. Online Marketing might want to consume some of this data for targeting purposes. For eg., you may want to send emails to all users who have bought books/mobiles and who reside in particular pincodes. Order related data is stored in the Order Management System (OMS), while user related data is stored in User Service. Using the API for bulk and complex queries can be inefficient. This is an example where you want to join two different sources. There are cases where we might want to make joins across 7-8 sources.<br />
To solve this problem, we're using MariaDB 10.0&#8242;s multi source replication. Each source maintains a lot of tables that is internal to their system. The marketing mysql slave instance selectively replicates from these sources so that it can consume relevant data for targeting.
<p>-How do I do it?<br />
<span id="more-204"></span><br />
There are multiple ways to do this. Here is a good post covering some of the ways:<br />
<a href="http://www.mysqlperformanceblog.com/2007/11/07/filtered-mysql-replication/" title="Partial Replication" target="_blank">http://www.mysqlperformanceblog.com/2007/11/07/filtered-mysql-replication/</a></p>
<p>- Can you add/remove tables/db to existing replication?<br />
Yes! Follow the following steps:<br />
1) Run <code language="sql">stop slave</code> on the slave where you want to add the tables/db. Let's call this target slave.<br />
2) Using method #3 from this post, take the dump of the required data.<br />
3) Using the replication parameters captured in step 2, use the following command to update the target slave:<br />
<br />
<code language="sql">
START SLAVE UNTIL MASTER_LOG_FILE='&lt;log_file&gt;', MASTER_LOG_POS=&lt;log_position&gt;; 
</code>
<br />
The above command starts replication till the 'log_position' and then stops the slave itself.<br />
4) Now, load the dumped data on the target slave.<br />
5) If you're replicating using replicate-do-db or replicate-do-table, add the new tables to the conf file and restart the target slave.<br />
6) Run <code language="sql">start slave</code> on the target slave and watch the new tables getting the new data! :) </p>
<p>- Is it safe?<br />
Well, yes and no. It works just like normal replication so yes, it is safe. No, in terms of the slight increase in chance of breaking replication.<br />
Say you are replicating only table tA, tB and tC from DB dbA. Now, if a DML command is run which uses cross join of dbA and dbB to update data from table tA, then yes, replication may break if you're using statement based replication. </p>
<p>- If I am using slave status to find the replication parameters, then which slave parameters should I consider?<br />
For replication to start, the mysql instance needs the name of the log file it needs to read, and the position at which it should start reading from this file. For a slave, there is a difference between the IO thread reading the binlogs and the SQL thread executing the commands on the existing slave.<br />
The <code language="sql">show slave status</code> command has three log files and there might be confusion as to which log file to use.<br />
If you are taking the dump from an existing slave, the data that the current slave holds depends on the position of the SQL thread running and not the IO thread.<br />
Mapping this to the "show slave status" command, the only parameters you should consider are Relay_Master_Log_File and Exec_Master_Log_Pos. </p>
<p>Hope this helps! If you have any further questions on MySQL partial replication, do let me know.</p>