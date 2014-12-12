---
layout: post
title: MariaDB seconds behind issue fixed
date: 2014-03-10 15:28:38 -0500
date_formatted: March 10, 2014
comments: true
categories: [mysql, mariadb, opensource, tech]
---

We (my friend Vishnu and I) had reported a bug with MariaDB 10.0.4. Just got an update that the issue is fixed now! Here are the details about the bug:
We had setup maria 10.0.4 for multisource replication. It was replicating from a Percona 5.5 master. When we started replication, Maria was a day behind its master. It was working fine, but we noticed this phenomenon:
When we execute <code language="sql">show all slaves status</code> consecutively, the 'seconds_behind_master' flips from 0 to XXX and XXX to 0.
Note: XXX is some positive number.

Reference: https://mariadb.atlassian.net/browse/MDEV-5114

The fix is deployed in MariaDB 10.0.10.