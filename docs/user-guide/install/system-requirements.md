---
layout: docwithnav
title: ThingsBoard system requirements
description: Hardware and software system requirements for ThingsBoard CE.
---

* TOC
{:toc}

## Overview

Some text goes here.

## Hardware Requirements

### Architecture

As a Java application, ThingsBoard is naturally **platform-agnostic**. Its official installation packages (.deb, .rpm) and Docker images are built to be architecture-independent, meaning they run correctly on both **x86_64** (AMD/Intel) and **arm64/aarch64** (ARM) systems.

### Bare metal single host sizing

This section assumes a single machine that runs only the ThingsBoard application and PostgreSQL with no other components included.

| Profile     | Typical use case                                          | vCPU |   RAM | Storage                                                                                                                                  |
| ----------- | --------------------------------------------------------- | ---: | ----: | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Minimal     | Evaluation or development with low message rates          |    2 |  4 GB | Minimum 10 GB of free space. SSD recommended; for evaluation on Raspberry Pi, an SD card may be used. Maintain utilization < 90 percent. |
| Recommended | Production deployment handling moderate to high workloads |    4 | 16 GB | Minimum 10 GB of free space. Use SSD or NVMe. Size for telemetry retention. Maintain utilization < 90 percent.                           |

{% capture 4gb_ram_notice%}
Heap note: on 4 GB RAM hosts, [cap the ThingsBoard JVM heap at 2 GB](/docs/user-guide/install/ubuntu/#step-5-optional-memory-update-for-slow-machines-4gb-of-ram) using `-Xms2G -Xmx2G` to leave memory for PostgreSQL and the OS.
{% endcapture %}
{% include templates/info-banner.md content=4gb_ram_notice %}

The table provides a general reference for hardware and software planning. Actual requirements depend on deployment scale, message rate, and data retention.  

For practical benchmarks and deployment examples, refer to:

- [ThingsBoard performance on different AWS instances](/docs/reference/performance-aws-instances/)
- [IoT platform deployment scenarios](/docs/reference/iot-platform-deployment-scenarios/)
- [ThingsBoard data collection performance](/docs/reference/performance/)

<!-- TODO: check if Windows exe installation supports ARM -->

We recommend adjusting these parameters depending on your server resources. It should be set to at least 2G (gigabytes), and increased accordingly if there is additional RAM space available. Usually, you need to set it to 1/2 of your total RAM if you do not run any other memory-intensive processes (e.g. Cassandra), or to 1/3 otherwise. -->

## Software Requirements

This section lists the operating systems, runtime environments, and third-party software verified with the **ThingsBoard {{ site.release.ce_full_ver }}**.  
Older releases may have different requirements or version compatibility.

### Supported operating systems

ThingsBoard supports the following operating systems:

- Ubuntu (22.04 LTS / 24.04 LTS)
- CentOS (8/9, RHEL 8/9)
- Windows (11)
- macOS (Sonoma, Sequoia, Tahoe)
- Raspberry Pi OS (Debian 13)

{% capture other_os_notice%}
Installation of ThingsBoard on other operating systems is possible, but is not recommended or supported.
{% endcapture %}
{% include templates/info-banner.md content=other_os_notice %}

## Third-party software

ThingsBoard requires **Java 17** as its runtime environment.  
Other components listed below are recommended and tested with the latest ThingsBoard release.  
Earlier major versions of these components are generally compatible if still supported by their vendors.

| Component          | Description                                                                                                  | Recommended version |
| ------------------ | ------------------------------------------------------------------------------------------------------------ | ------------------- |
| **Java (OpenJDK)** | **Required.** Core runtime environment used to run the ThingsBoard server application.                       | **17.x**            |
| **PostgreSQL**     | **Required.** Relational database used to store entities, relations, and telemetry data.                     | **17.x**            |
| **Cassandra**      | **Optional.** NoSQL database for time-series and high-volume telemetry data storage.                         | **5.x.x**           |
| **Kafka**          | **Optional.** External message queue service.                                                                | **4.x.x**           |
| **Valkey**         | **Optional.** In-memory data store used for caching and lightweight queueing. Drop-in replacement for Redis. | **8.x**             |
| **HAProxy**        | **Optional.** TCP/HTTP load balancer and reverse proxy used for HTTPS termination.                           | **2.9**             |

<!-- explain in more details why would you need this component and when -->

## Network Requirements

ThingsBoard does not have strict network performance requirements and can work in air-gapped environments.  
The following ports are used by ThingsBoard for web access and device communication.  
You can keep ports for unused transport protocols closed, including the HTTP port if reverse proxy is used instead.

- **8080 (TCP)** – HTTP Web UI and REST API access  
- **1883 (TCP)** – MQTT endpoint  
- **8883 (TCP)** – MQTT over SSL (secure MQTT)  
- **7070 (TCP)** – Edge RPC communication  
- **5683–5688 (UDP)** – CoAP and LwM2M protocols  
- **443 (TCP)** – HTTPS (when using a reverse proxy such as HAProxy)


<!-- Thingsboard PE requires internet access (do not mention here, it is CE documentation) 
The server must have internet access in order to make check license requests. You need to allow your server access to the license.thingsboard.io
-->

## Deployment Environments

ThingsBoard can run as a single monolith application or as a set of microservices.

In the **monolithic deployment**, all Thingsboard components run inside a single Java process and share the same system resources. This option uses the least memory and is recommended for development, prototyping, and small production deployments.

More details are available in [Monolithic deployment](/docs/reference/monolithic/).

In the **microservices deployment**, core Thingsboard components such as transport, tb-node (core, rule engine), web UI, and JavaScript executors are deployed as separate services and communicate through Kafka and Zookeeper. This option is designed for horizontal scaling, high availability, and easier maintenance of individual services. 

More details are available in [Microservices deployment](/docs/reference/msa/).

For a detailed overview of ThingsBoard architecture, core services, message queues, and deployment models, including clustering, scaling, and communication between components, see [Reference architecture](/docs/reference/).

To explore available deployment options and learn how to set up ThingsBoard in different environments, see [Installation options](/docs/user-guide/install/installation-options/).

## Next steps

{% assign currentGuide = "InstallationGuides" %}{% include templates/guides-banner.md %}