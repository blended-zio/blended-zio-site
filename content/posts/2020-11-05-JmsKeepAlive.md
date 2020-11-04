---
title: Keep alive for JMS connections
date: "2020-11-05"
tags:
  - ZIO
  - Streams
  - JMS
categories:
  - ZIO
Summary: Add a simple keep alive mechanism to (JMS) connections

amqsrc:       "modules/blended-zio-activemq/blended-zio-activemq/src/main/scala"
streamssrc:   "modules/blended-zio-streams/blended-zio-streams/jvm/src/main/scala"
streamstest:  "modules/blended-zio-streams/blended-zio-streams/jvm/src/test/scala"
---

# Why do we need a keep alive

In our existing application we have noticed that in some circumstances a connection that still exists and is reported as _connected_ within the monitoring tools does not work as expected. In the case of JMS this usually happens when a particular connection is only used

{{< hint info >}}
The complete source code used in this article can be found on [github](https://github.com/blended-zio/blended-zio-streams)
{{< /hint >}}

## A simple Keep Alive monitor

## Using the Keep alive monitor with JMS connections