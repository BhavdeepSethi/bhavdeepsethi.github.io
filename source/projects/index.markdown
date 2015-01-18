---
layout: page
title: "projects"
date: 2014-06-16 18:11
comments: true
sharing: true
footer: true
---

<p>Here is a list of some of the big projects I've worked on along with a short description about each of them.</p>

<hr >
<p><font color="#1F7DE0"><br />
<h3><u>Email Intelligence System </u></h3></font><br />
<b>Context:</b><br />
My first Machine Learning/Natural Language Processing project at <a href="http://www.ccls.columbia.edu/" >CCLS</a>, Columbia. I was working on the "Aboutness" module in this project. Given an email corpus, can we identify privilege/sensitive documents present? Also, we need to provide a small log/summary mentioning what the email is about. We had to make sure that the summary of the priviledge mails do not reveal any sensitive information. Major components used were Weka (running different ML algorithms), StanfordCoreNLP (CoRef analysis, NounPhrase extraction), MALLET (for string Kernels), SVM-Rank.

<hr >
<p><font color="#1F7DE0"><br />
<h3><u>SkilLender </u></h3></font><br />
<b>Context:</b><br />
As part of final project of the Cloud &amp; Big Data course, we (team of three) had to develop a web app leveraging Amazon Web Services (AWS). We built a marketplace where people could list their skills along with its competency. Using Facebook SDK, we built an internal skill graph. People could mention the degree of graph they want to access while searching. Say, show me friends of friends who know how to code in Python. Or you can find people who are, say looking to learn piano. You two can pair up and learn it together!
The web app was written in <a href="http://expressjs.com/">Express</a>. The data store was <a href="http://aws.amazon.com/rds/">RDS</a>. The graph was stored in <a href="http://neo4j.com/">Neo4J</a>. The rest service was written with <a href="https://jersey.java.net/" >Jersey</a>. The app was hosted with <a href="http://aws.amazon.com/ebs/">Amazon EBS</a>.We were using <a href="http://aws.amazon.com/sqs/">SQS</a>, <a href="http://aws.amazon.com/ses/">SES</a> in the back end for building the graph asynchronously and sending out emails.  

<hr >
<p><font color="#1F7DE0"><br />
<h3><u>User Insights / Sniper </u></h3></font><br />
<b>Context:</b><br />
The last project I worked on during my Flipkart stint. This platform offered a 360 degree view of Flipkart visitors, enabling relevant and intent based experiences driving engagement, loyalty and personalization. For eg. Given the first name of the user, can we derive the user's gender? What is the user's category affinity? What is the user's time affinity? What time does he open his emails, uses the app, browses the site, etc.? Is the customer an offer seeker or is he concerned about SLA? Can we identify resellers among the horde of normal customers of FLipkart? All this information will enable us to provide a very personalized customer experience. The application mines data in the order of 100 TB encompassing sources like logs, relational DBs and NoSQL stores. The primary store utilized is HBase and the processing is done mostly through Yarn. The workflows are managed through Azkaban and the logical representation of data varies from key value, columnar to User graphs.</p>

<p><b>Challenges:</b><br />
Data. Considering the scale at which Flipkart works, we generate a lot of data. Plus, most of the data resides in disparate sources. Collating all this data, normalizing it, understanding it, building models around it and generating insights/targeting capabilities are just a few of the challenges faced as of now. We're in the process of deploying Mahout to enable learning on this data.</p>
<p>Speed. We had two pipelines. Fast one for pulling data from relational stores. Slow for processing the access logs in orders of TB. The jobs still shared common hardware. There were times where the slow pipeline used to run for days and the backlog just kept growing. </p>
<p>Failures. Hadoop jobs fail. To support resumability as well as idemptency, we were maintainging a lot of data states in Hbase. This required regular compaction on that table which was basically a downtime mode for us. Bad idea.</p>


<p><b>Solution:</b><br />
Because of the above challenges, we re-arched the design and moved to something resembling a lambda-architecture. We introduced Kafka which was responsible for maintaining our fast/slow pipelines. We introduced Storm for doing real time insights. The batch jobs were still handled by Haddop. But the major bottleneck of maintainging states in Hbase could be avoided. The jobs which used to take days was finishing in order of 2-3 hours. 

<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Communication Service </u></h3></font><br />
<b>Context:</b><br />
Communication Service is a unified service responsible for any communication with the internal/external customers of Flipkart. This includes sending SMS/Email/Notifications to the customers, sellers as well as internal employees of Flipkart. This includes providing real time reports with respect to messages sent, the success rate, failure rate, open rates, click rates across multiple pivots like campaign/client/message. </p>
<p><b>Challenges: </b><br />
The same service is used to deliver transaction and marketing emails. Both these use cases are very different.<br />
Transaction emails are distributed throughout the day, have a predictable volume and require delivery guarantees. Reliability is more important than performance.<br />
Marketing emails are sent in short bursts in very high quantity, the volumes vary drastically depending on days and time. Here performance is much more important than reliability. We have to ensure we can send send out 2-3 million emails within a very short time. </p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Workflow Engine &#038; Ceryx </u></h3></font><br />
<b>Context: </b><br />
Ceryx is the email marketing platform that we built for Flipkart. This platform is used for generating the static/dynamic emails and create the target group for the emails. The targeting module developed for this is used by other teams for defining customer segments as well. The underlying platform is a generic event based workflow engine. Once this platform matures a bit, we plan to open source it. </p>
<p><b>Challenges: </b><br />
How do we develop a platform (Workflow Engine) that can be leveraged by other teams in Flipkart as well? How do we ensure the customer receives the best content at the best time when there are multiple emails need to go out in the day? How do we ensure we do not end up spamming the users? Given some content, what is the best customer base that the emails should go out to? How to generate over 10 million personalized emails in a very short time? How to make this process asynchronous? How to enable A/B testing for a particular content going out?</p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Digital Reporting Platform </u></h3></font><br />
<b>Context: </b><br />
A Reporting module for the Digital Platform, which involved Async data collection, Queryability, BISG Report generation, Notification system for clients and Self Healing.This included the ability to generate customized reports for the clients. </p>
<p><b>Challenges: </b><br />
How do you ensure data sanity for different dynamic channels? How do you ensure the platform has the ability to identify and correct the data itself? How often do you crawl different websites to ensure the exchange rates are the latest? </p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Digital Recommendation Module </u></h3></font><br />
<b>Context: </b><br />
Implemented the Recommendation Module for mp3 and eBooks using collaborative filtering techniques like Item-Item Recommendation and Content Based Classification.</p>
<p><b>Challenges: </b><br />
Collaborative filtering works fine for popular content because of buying/browsing patterns. How do you solve cold start problem? How to promote new releases via recommendation? Content based classification will always give more weight to other content by the same artist. How do you recommend content whose metadata does not have anything in common with other content? How do you leverage <a href="http://marsyas.info/" >Marsyas</a> to ensure true content based classification for mp3 content? </p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Digital Service Platform </u></h3></font><br />
<b>Context: </b><br />
Developed the service responsible for all Content and Meta ingestion of digital products on Flipkart.com. The service was also powering all mp3 downloads via website, desktop and mobile apps.</p>
<p><b>Challenges: </b><br />
How to ensure ingestion platform is compatible with different ingestion mechanisms? Some will push content to you, some will ask you to pull. Some will transfer over http, some will support ftp. Metadata format will vary from XML, JSON, CSV to even MS Excel! Download service needs to be scalable and have very low latency. How to restrict number of downloads at an order/device/ip level? How to make this restriction configurable based on the content being served? </p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Content API </u></h3></font><br />
<b>Context: </b><br />
Designed and developed the core APIs for providing meta and content ingestion access to other publishers (Gaana.com, Dhingana.com, in.com, Saregama, etc.) taking care of Authentication, Content Expiry and GeoIP restrictions.</p>
<p><b>Challenges: </b><br />
Security was the biggest concern here. How do you prevent URL tampering? How do you ensure nobody gets unrestricted access to your content? How do you ensure the url can be used a certain number of times or for a certain period only? Download requests being served from CDN are fine, but streaming data has to be encrypted on the fly. How do you ensure your servers don't crash with the request load? GeoIP restrictions at the content/publisher/owner level? How to ensure the seamless switch to the DR location with content and meta?</p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Content Management System (CMS) </u></h3></font><br />
<b>Context: </b><br />
CMS is the backbone of any delivery system. Designing a proper schema, taking into account the dynamic nature of the attributes helps adding new data about the content with ease. The CMS in question here, dealt with Audio data.  </p>
<p><b>Challenges: </b><br />
How do you design a relational schema when new attributes about songs/albums/artists keeps on changing everyday. Especially with the Indian Music industry, which has not kept pace with technology. The most challenging aspect here was to extend the existing CMS to Video Content, White Label Sites and Mobile Apps.</p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Business Intelligence Platform </u></h3></font><br />
<b>Context: </b><br />
Designed and developed the business intelligence platform providing relevant metrics via channels like reports, dashboards to content owners and content publishers.</p>
<p><b>Challenges: </b><br />
We were using the same machine for reporting as for production data. That meant that a heavy load on reporting could impact API latencies. Since the content owners/publishers could query data on any date range across multiple pivots, how do you ensure an optimal design which has the least DB query footprint. </p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Android Apps </u></h3></font><br />
<b>Context: </b><br />
I developed Android Apps for encrypted music streaming over http for the clients of MIME360. One of these Apps became the prototype for the first Flipkart Android app, Flyte App v1. </p>
<p><b>Challenges: </b><br />
Performance. Since the app worked on encrypted data, the player had to be built within the app. How do you ensure a native player experience with all standard set of the player features expected? </p>
<hr />
<p><font color="#1F7DE0"><br />
<h3><u>Sensor Based Security System </u></h3></font><br />
<b>Context:</b><br />
We developed a functional real time Intrusion Detection System using infrared sensors and a regular webcam that takes care of the security of private offices or any room that contains sensitive data.<br />
On detecting an intrusion, a message and an email with a link is sent to the owner through which he can access a live feed of his webcam.<br />
Also the video is stored on an FTP server and his local drive for future references.</p>
<p><b>Challenges: </b><br />
How to create a very reliable and secure IDS system while minimizing the cost at the same time? The IDS had to be made with equipment found around the house or at least with items which were easily available. </p>
<hr />