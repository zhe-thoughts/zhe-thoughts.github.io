---
layout: post
title: Run a Hadoop Cluster from IntelliJ
---

As part of [HDFS-10534](https://issues.apache.org/jira/browse/HDFS-10534), which involves a web interface change, I spent a few hours today finding a way to quickly verify the UI change with different patch versions.

### Basics
A typical Hadoop cluster should have HDFS and YARN servers. On the HDFS side we should start 1~2 ```NameNode``` server(s), depending on whether HA (high availability) is enabled, together with several ```DataNode``` servers. On the YARN side we should start the ```ResourceManager``` and a few other services.

I only tried actually starting the HDFS part, and will describe the experience in the rest of the post. It turned out not as simple as ```Run``` the servers one by one from IntelliJ.

### CLASSPATH

### Configurations
The first thing to do is to put a ```hdfs-site.xml``` configuration file on the class path. It should at least specify the default filesystem URI:

```
<configuration>
  <property>
  <name>fs.defaultFS</name>
  <value>hdfs://localhost:9911/</value>
  </property>
</configuration>
```

### Dependencies

