# Blended ZIO

## About this project

For now this is a personal project to learn about FP by migrating (rewriting) a real world application with an FP approach. I have decided to document the rewrite as much as possible, so others may benefit from the experience and to have a reference in case I want to ask questions on any of the FP related forums.

Even though it is my intent to complete the migration, paid projects might take precedence.

### Heritage

This project is based on [Blended](https://github.com/woq-blended/blended), an integration framework implemented on top of Scala 2.13, Akka, Akka Http and Akka Streams. It is currently in it's 3rd productive generation.

The first generation of Blended has been implemented in Java and has used [ActiveMQ](http://activemq.apache.org), [Karaf](http://karaf.apache.org/) and [Camel](https://camel.apache.org/) as core components. Once version 1 went into mainteance, we started to refactor the tests to Scala and with version 2 we decided to move to Scala entirely for all our self written components.

Also with version 2 we finished our integration test framework to automatically spin up our test environments in docker and use docker based integration tests.

With version 3 of the framework we have replaced Karaf with our own OSGi container framework written in Scala and all Camel based integration flows have been rewritten to Akka Streams.

### Moving forward

Even though the code base of _Blended_ is fairly robust and tested, there are some *dark* areas within the code base where nobody wants to go and much less make any changes. Having looked at several approaches how these areas can be addressed we feel that if there is a next version of the _Blended_ framework it should be moving towards an FP implementation.

This is easier said than done, but nevertheless I have created the _Blended ZIO_ project as a personal playground to achieve several things:

* The code base of _Blended 3_ is not functional at all. It uses untyped actors under the covers which is one of the pain points when refactoring is required. The developer has to understand the interface in terms of which messages may be sent to the actors and which are the responses, but the type system and the compiler are of little to no help in understanding these interfaces. _Blended ZIO_ on the other hand shall be as functional as possible.

  I have given some thought how to get from _Blended 3_ to _Blended ZIO_ and have weighed _refactor_ against _rewrite_. After going through quite a bit of documentation, asking on the forum etc. I am leaning towards a rewrite. This decision may swing into the other direction again, but even as I consider myself an to be an experienced (Scala) developer I feel that refactoring towards to ZIO might require me to refactor modules which I am going to rewrite sooner or later anyway.

* _Blended 3_ lives in a single repository and has grown to more than 60 modules within that repo. While that is convenient for refactoring on the global project level, the entire build process sometimes lacks flexibility. The latter has been addressed by moving the build to [Mill](http://www.lihaoyi.com/mill/), but still the build process feels somewhat complicated. With Blended ZIO, the modules shall become smaller and concentrate on a specific functionality.

* _Blended 3_ uses [OSGi](https://www.osgi.org/) as it's runtime within the JVM. Looking at [ZIO](https://zio.dev/) and it's ecosystem the first thing to note is that there is no OSGi support out of the box. So the question rises (again) whether _Blended_ shall stick to OSGi or not. At the end of the day OSGi was chosen because when _Blended_ was created, which was before today's virtualization techniques where available. We did need a runtime allowing in place updates of the deployed jars via a management API.

  As of version 3, the management API is read-only, the requirement for in place updates has vanished in favor of bundling the _Blended_ based applications statically and use [Ansible](https://www.ansible.com/) to roll out software updates - replacing and restarting the entire JVM. As a result, OSGI is merely used to maintain the dependencies between various services within the JVM to make sure everything is wired up correctly.

  From my understanding, within the ZIO ecosystem there are different options which at least deserve a closer look. One of the options is the [distage framework](https://izumi.7mind.io/distage/) which seems to promise most of the requirements we might have in our packaging.



