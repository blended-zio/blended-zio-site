---
title: Using ZIO Logging
date: "2020-09-28"
tags:
  - ZIO
  - Logging
  - Utilities
categories:
  - ZIO
Summary: Keep business interfaces clean of non-functional requirements such as loggin, but still allow to use them in service instances.
---

# Use ZIO logging

Within _Blended ZIO_ the service are kept clean of non functional requirements such as relying on a logging service being present within the environment.

For example, the `Service` within `MBeanServerFacade` is defined as follows.

{{< codesection file="modules/blended-zio-jmx/ziojmx/jvm/blended/zio/jmx/MBeanServerFacade.scala" section="service" >}}

However, within the service's implementation `JvmMBeanServerFacade` the corresponding methods leverage the API of [zio-logging](https://zio.github.io/zio-logging/) to produce some output while executing the effects.

{{< codesection file="modules/blended-zio-jmx/ziojmx/jvm/blended/zio/jmx/MBeanServerFacade.scala" section="info" >}}

So, when we assemble the service

* we need to provide a Logging service when building up the ZLayer
* we need to make the Logging service available to the service implementation
* the business service as such should not have any knowledge of the Logging service requirement

The code to construct the live service which requires `Logging` leverages `ZLayer.fromFunction`. We see that a `Logging` service is required within the environment and we can use the parameter to the `fromFunction` call in the `provide` operator so that the requirement of having a `Logging` service is eliminated and the sole business service interface remains.

{{< codesection file="modules/blended-zio-jmx/ziojmx/jvm/blended/zio/jmx/MBeanServerFacade.scala" section="zlayer" >}}

We might have other service implementations that do not require logging or use a different logging API while keeping the same business interface.

Finally, we can construct the environment for our program as we do in the testcase:

{{< codesection file="modules/blended-zio-jmx/ziojmx/test/jvm/blended/zio/jmx/MBeanServerTest.scala" section="zlayer" >}}


