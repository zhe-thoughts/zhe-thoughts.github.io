---
layout: post
title: Why Kerberos? Simple Explanation in the HDFS Context 
---
For people without a security background, Kerberos can be very hard to understand. I recently had to fix a number of Kerberos issues in HDFS, and that turned out to be very helpful for my education.

From a high level, every process should represent *someone* who is entitled to access resources and request services (*principle* in Kerberos terminology).
