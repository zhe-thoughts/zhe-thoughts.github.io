---
layout: post
title: Why Kerberos? Simple Explanation in the HDFS Context 
---
For people without a security background, Kerberos can be very hard to understand. I recently had to fix a number of Kerberos issues in HDFS, and that turned out to be very helpful for my education.

From a high level, every process should represent *someone* who is entitled to access resources and request services (*principle* in Kerberos terminology). The way it proves to a resource / service provider is to show a *token*. A token is a chunk of bytes that is hard to generate but easy to verify. Kerberos ```KDC``` is in charge of distributing tokens of this sort.

As a concrete example, in a Kerberized cluster, ```Balancer``` acquires a token from ```KDC``` saying that it represents the cluster administrator. It then uses the token to check with ```NameNode``` about block locations, and work with ```DataNode```s to move those blocks. 
