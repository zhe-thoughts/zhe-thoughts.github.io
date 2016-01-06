---
layout: post
title: Java byte array vs. ByteBuffer
---

This is a quick note comparing arrays and (direct / indirect) ```ByteBuffer```. So basically, ```Buffer``` is an abstraction using which to prepare data for channel-based I/O. ```ByteBuffer``` is a subtype offering byte-oriented operations.

The ```ByteBuffer``` API can be implemented by either wrapping a byte array (non-direct ```ByteBuffer```) or allocating from off-heap memory (direct ```ByteBuffer```).

Here's the easiest way for me to understand ```ByteBuffer```: 1) we introduced a way to operate on off-heap memory called ```DirectByteBuffer```; 2) to unify its handling with on-heap byte arrays we introduced an abstraction called ```ByteBuffer```.

{% include twitter_plug.html %}