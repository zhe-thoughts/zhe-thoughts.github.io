---
layout: post
title: Impact of Large Requests in Shared Services
---

# Impact of Large Requests in Shared Services
## Tl;DR
When an abnormally large request (takes *L* seconds to process) is issued to a shared service with frequency of *F*, its impact on the service overall performance grows in *square* relationship with *L* and also linearly with *F*. So, break up large requests as much as you can!

## Problem Statement
Hadoop uses a single metadata server (`NameNode`) to keep track of all files and blocks in a cluster. The `NameNode` in a large Hadoop clusters is usually very heavily loaded — handling 10s of thousands of RPC requests per second.  It is therefore (arguably) the most likely source of cluster performance issues.

From multiple severe cluster performance degradations, we have identified the root cause to /very infrequent, but very expensive/ requests to the `NameNode`.  Last year, my colleague Konstantin optimized HDFS `Balancer` ’s  `GetBlocks` call to `NameNode` from 40 ms to a few ms (see [HDFS-11634 Optimize BlockIterator when iterating starts in the middle.](https://issues.apache.org/jira/browse/HDFS-11634)  for details). Although `GetBlocks` is only called a couple of times a second, this optimization very dramatically improved the average time `NameNode` can serve an RPC.

I have since been puzzled by this seemingly unproportional impact of infrequent-but-large requests. Think about it, if you make a 40 ms call twice a second, you would be “occupying” *8%* of the “total service capacity”, right?

What finally triggered me to spend some more time thinking through this question was last week’s cluster incident, where a a super expensive RPC request every 2 mins, each taking 10 seconds to process. Average request latency increased from a few ms to *500*!

## Intuitive Explanation
Take the above case (a 10 second request every 2 mins) as example. Let’s further assume:
	1. Every second the server processes *N* “normal” requests
	2. Every “normal” request takes *M* seconds to process. In a healthy, non-saturated system, `MxN` should be much smaller than 1.

At the end of a large, 10-second request, `10xN` normal requests would be “blocked”.  Among those blocked requests, the average “blocked time” is 5 seconds. This happens in every 2 mins (or 120 seconds). Therefore those `10xN` blocked requests should be roughly 8.3% of all normal requests. In another words, this large request is causing *8.3% of all requests to have a service time of 5 seconds*. This by itself is enough to bring the overall service time from almost-zero to *416 ms*, which pretty closely matches with observation from the cluster incident.

Were this 10-second request to become 20 seconds, the overall service time would *quadruple*, because twice as many normal request would be blocked, with twice as much average block time.  Were the frequency to increase from once 2 mins to once a minute, the overall service time would double.

Of course the above intuitive explanation is pretty rough. It has not considered the arrival distribution of the normal requests (in most cases should be a Poisson process). More importantly, it has ignored the compounding effect where a blocked normal request that arrived earlier would increase the wait time of a blocked normal request that arrived later. 

## Simple Mathematical Explanation
Per [Pollaczek–Khinchine formula - Wikipedia](https://en.wikipedia.org/wiki/Pollaczek%E2%80%93Khinchine_formula), the average wait time in a service queue can be expressed as:

<img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/8775d3fff838b07d162796199fbffdad0c6a0354"/>

, where `Var(S)` is the variance of the service time distribution `S`. In the situation being discussed here, the service time of the large requests *L* is several magnitudes higher than that of normal requets, therefore the variance of `Var(S)` is dominated by the square of `L` itself.
