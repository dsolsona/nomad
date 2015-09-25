---
layout: "docs"
page_title: "Drivers: Java"
sidebar_current: "docs-drivers-java"
description: |-
  The Java task driver is used to run Jars using the JVM.
---

# Java Driver

Name: `java`

The `Java` driver is used to execute Java applications packaged into a Java Jar 
file. The driver currently requires the Jar file be accessible via
HTTP from the Nomad client. 

## Task Configuration

The `java` driver supports the following configuration in the job spec:

* `jar_source` - **(Required)** The hosted location of the source Jar file. Must be accessible
from the Nomad client, via HTTP

* `args` - (Optional) The argument list for the `java` command, space separated. 

## Client Requirements

The `java` driver requires Java to be installed and in your systems `$PATH`.
The `jar_source` must be accessible by the node running Nomad. This can be an 
internal source, private to your cluster, but it must be reachable by the client 
over HTTP. 

## Client Attributes

The `java` driver will set the following client attributes:

* `driver.java` - Set to `1` if Java is found on the host node. Nomad determines
this by executing `java -version` on the host and parsing the output
* `driver.java.version` - Version of Java, ex: `1.6.0_65`
* `driver.java.runtime` - Runtime version, ex: `Java(TM) SE Runtime Environment (build 1.6.0_65-b14-466.1-11M4716)`
* `driver.java.vm` - Virtual Machine information, ex: `Java HotSpot(TM) 64-Bit Server VM (build 20.65-b04-466.1, mixed mode)`

## Resource Isolation

The resource isolation provided varies by the operating system of
the client and the configuration.

On Linux, Nomad will attempt to use cgroups, namespaces, and chroot
to isolate the resources of a process. If the Nomad agent is not
running as root many of these mechanisms cannot be used.

As a baseline, the Java jars will be ran inside a Java Virtual Machine,
providing a minimum amount of isolation.
