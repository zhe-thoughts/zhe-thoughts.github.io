---
layout: post
title: Note: AMPLab Winter Retreat
---
UC Berkeley AMPLab generously hosts annual retreats to present early-stage research work. It also serves as a venue for collaborators from the acdemia and industries to exchange ideas. This year's winter retreat was held at the Granlibakken resort in Tahoe City. Very good ski resort for beginners and families with young kids.

This is my summary from attending the event:

**New Lab**: The most interesting topic was the annoucement of a new lab to succeed the current AMPLab. From my understanding, this succession includes a new name (to be determined) and a new theme. Ion Stoica presented a number of visions under three highlighted points:

1. Decision latency
2. Data freshness
3. Strong security

Ion used the example of *stock trading* and envisions a broader range of applications to match the msec-level data freshness and microsec-level decision latency in the future. Nagivation and *auto-driving* was used as a second example, where the improved data freshness and decision latency could make a huge impact. 

Their goal is to create a general platform -- can be seen as next Spark -- to supplement these three properties to upper level applications. Ion pointed out a few challenging trade-offs between conflicting goals, and drew a spectrum to position current systems in these trade-offs. *Hadoop-based solutions* was categorized as good-latency, bad-freshness, bad-security, OK-functionality. *BDAS-based solution* got similar scores except for OK-freshness, and somewhat better functionalities. So security has been identified as a common weak point. Despite storage-level encryption, many places in the compute pipe are unprotected. There are some security solutions to support computation on encrypted data (e.g. CryptDB, Mylar) but they only support simple algorithms. 

So their goal is to win along every dimension on the spetrum ("Spark-like functionality with 100x lower latency on 10x fresher data, with strong security").

**[Succint](https://amplab.cs.berkeley.edu/publication/succinct-enabling-queries-on-compressed-data/)**: Several talks and posters were dedicated to Succint, an in-memory compression format. The project already has a published paper so I won't go into too much details. The presentation was of high quality and worth watching. First time for me to learn that Succint optimizes for *point queries* while sacrificing scan-based queries. This means users have to determine their workloads before-hand and decide whether to use Succint. Succint-Spark package is already GA. The current plan is to enable more features, including regex search, graph search and so forth. They are also building an enryption-compression solution. The technique is mini-batching (compress and encrypt a number of rows) with some empirical parameter tuning.

**Machine Learning**:

{% include twitter_plug.html %}
