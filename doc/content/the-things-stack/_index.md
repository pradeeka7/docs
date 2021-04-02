---
title: The Things Stack
description: The Things Network is upgrading to The Things Stack V3
aliases:
 - /the-things-stack-v3/
weight: 700
image: "network/icon.png"
menu:
  main:
    weight: 20
---

As announced during The Things Conference held in January 2021, The Things Network V2 software is upgrading to The Things Stack, i.e. the free LoRaWAN network operated by The Things Network community now runs The Things Stack Community Edition. This upgrade comes with a set of brand new features, out-of-the-box integrations, extended coverage and improved user experience. Read along to find out everything about it. 

Read along to learn what The Things Stack (Community Edition) is, why it is better than the previous version and how to migrate your gateways and devices to it.

## What is The Things Stack (Community Edition)?

**<a href="https://www.thethingsindustries.com/docs/" target="_blank">The Things Stack</a>** (also know as V3 among The Things Network community) is the version 3 of the LoRaWAN network server that is being developed and maintained by <a href="https://www.thethingsindustries.com/" target="_blank">The Things Industries</a>.

The Things Stack is a user-friendly, enterprise-grade LoRaWAN network server built on top of an open-source stack. Comparing to The Things Network V2, The Things Stack is not just an update but a completely new solution built from scratch. 

The Things Stack <a href="https://www.thethingsindustries.com/docs/reference/components/" target="_blank">architecture</a> shown below follows the LoRaWAN Network Reference Model. 

![Network Architecture](architecture.png)

The Things Stack is available under several <a href="https://www.thethingsindustries.com/docs/download/" target="_blank">distributions</a>, depending on which deployment type you prefer - on cloud, on premises, private, public, SLA-backed or not, DIY, etc. 

The free, community-operated LoRaWAN network, formerly known as The Things Network V2, now runs **The Things Stack Community Edition (V3)**, so this documentation contains the information relevant only for that distribution.

> For detailed information on The Things Stack and all available distributions, visit <a href="https://www.thethingsindustries.com/docs/" target="_blank">The Things Stack official documentation page</a>.

## How can I start using The Things Stack Community Edition?

The Things Stack Community Edition, which is a free edition of The Things Stack, can be accessed via the Console. You can access the Console by <a href="https://console.cloud.thethings.network/" target="blank_">selecting a cluster</a> that is closest to you geographically.

![The Things Stack Community Edition Console](TTN-V3-console.png "The Things Network Console")

## Why should I migrate my devices and gateways from The Things Network V2 to The Things Stack Community Edition?

> Here, we assume you have experience using The Things Network V2. If you have not been using The Things Network V2 yet, you should visit the [Quick Start]({{< ref "quick-start" >}}) guide.

> All the information in this section refers to The Things Stack Community Edition, but also The Things Stack in general. For the purpose of simplicity of this section, we use the term *The Things Stack*.

The Things Stack, compared to The Things Network V2, is more scalable, more secure and supports all the latest LoRaWAN developments like the latest LoRaWAN versions 1.1 and 1.0.4. Also, near the end of 2021, The Things Network V2 clusters will be shut down. 

The Things Stack architecture is based on microservices which allows for better distribution of services, better scaling and interoperability with other LoRaWAN networks. 

The Things Stack supports all LoRaWAN classes (A, B, C) and multicast device groups, all existing LoRaWAN versions (including v1.0.4 and v1.1) and all regional parameters by defined by the LoRa Alliance. By being standards compliant, The Things Stack  allows passive roaming and will allow handover roaming in the future. Firmware updates over the air, advanced clustering and load balancing techniques also come along with this upgrade.

You will be able to reuse your username and password from The Things Network V2 to log in, thanks to The Things ID feature. Users can use the <a href="https://www.thethingsindustries.com/docs/getting-started/console/" target="_blank">Console</a> with an improved user interface, or <a href="https://www.thethingsindustries.com/docs/getting-started/cli/" target="_blank">CLI</a> to manage <a href="https://www.thethingsindustries.com/docs/gateways/" target="_blank">gateways</a>, <a href="https://www.thethingsindustries.com/docs/devices/" target="_blank">devices</a>, <a href="https://www.thethingsindustries.com/docs/integrations/adding-applications/" target="_blank">applications</a>, <a href="https://www.thethingsindustries.com/docs/getting-started/user-management/" target="_blank">users and organizations</a>, as well as to interact with uplink and downlink events in real-time. 

The advanced APIs offer gRPC, HTTP and MQTT <a href="https://www.thethingsindustries.com/docs/integrations/" target="_blank">integrations</a>. For debugging purposes, there are API-based event streams that can help you trace issues, monitor device behaviour and get useful alerts. <a href="https://www.thethingsindustries.com/docs/reference/data-formats/" target="_blank">Data Formats</a> used by The Things Stack have a different schema and have a much richer metadata support. For storing data, a <a href="https://www.thethingsindustries.com/docs/integrations/storage/" target="_blank">Storage Integration</a> is also available.

Users of The Things Stack will now have an opportunity to use the Global Join Server for storing and issuing security keys, and in that way, highly improve the security of their network. Since this network architecture is standards-compliant, developers can even use a join server operated by a trusted third party.

One of the most important features is the connection to <a href="https://www.thethingsindustries.com/docs/reference/peering/" target="_blank">Packet Broker</a>, allowing the exchange of traffic between The Things Stack Community Edition and private LoRaWAN networks which increases LoRaWAN network coverage, performance and capacity, and prolongs the end device battery life. 

> **For a more detailed comparison between The Things Network V2 and The Things Stack Community Edition, check out the <a href="https://www.thethingsindustries.com/docs/getting-started/migrating/major-changes/" target="_blank">Major Changes in The Things Stack</a> page.**

> **Continue reading to learn about [migrating your devices to The Things Stack Community Edition]({{< relref "migrate-to-v3" >}})**.

