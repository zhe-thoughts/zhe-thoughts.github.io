---
layout: post
title: Paper review: Tachyon (SoCC '14)
---

Tachyon is a distributed caching layer on top of disk-based file systems such as HDFS or GlusterFS. The core idea is to use lineage to provide lightweight fault tolerance. 

> More importantly, due to the inherent bandwidth limitations of replication, a lineage-based recovery model might be the _only_ way to make cluster storage systems match the speed of in-memory computations in the future. 