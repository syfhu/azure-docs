---
title: What is Azure Relay and why use it overview | Microsoft Docs
description: Overview of Azure Relay
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: ''

ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 05/02/2018
ms.author: sethm

---
# What is Azure Relay?

The Azure Relay service facilitates hybrid applications by enabling you to
securely expose services that reside within a corporate enterprise network to
the public cloud, without having to open a firewall connection, or require
intrusive changes to a corporate network infrastructure. Relay supports a
variety of different transport protocols and web services standards.

The relay service supports traditional one-way, request/response, and
peer-to-peer traffic. It also supports event distribution at internet-scope to
enable publish/subscribe scenarios and bi-directional socket communication for
increased point-to-point efficiency.

In the relayed data transfer pattern, an on-premises service connects to the
relay service through an outbound port and creates a bi-directional socket for
communication tied to a particular rendezvous address. The client can then
communicate with the on-premises service by sending traffic to the relay
service targeting the rendezvous address. The relay service then "relays" data
to the on-premises service through a bi-directional socket dedicated to each
client. The client does not need a direct connection to the on-premises
service, it is not required to know where the service resides, and the
on-premises service does not need any inbound ports open on the firewall.

The key capability elements provided by Relay are bi-directional, unbuffered
communication across network boundaries with TCP-like throttling, endpoint
discovery, connectivity status, and overlaid endpoint security.

The relay capabilities differ from network-level integration technologies such
as VPN, in that relay can be scoped to a single application endpoint on a
single machine, while VPN technology is far more intrusive as it relies on
altering the network environment.

Azure Relay has two features:

1. [Hybrid Connections](#hybrid-connections) - Uses the open standard web sockets enabling multi-platform scenarios.
2. [WCF Relays](#wcf-relays) - Uses Windows Communication Foundation (WCF) to enable remote procedure calls. WCF Relay is the legacy relay offering that many customers already use with their WCF programming models.

Hybrid Connections and WCF Relays both enable secure connection to assets that exist within a corporate enterprise network. Use of one over the other is dependent on your particular needs, as described in the following table:

|  | WCF Relay | Hybrid Connections |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **JavaScript/NodeJS** | |x |
| **Standards-Based Open Protocol** | |x |
| **Multiple RPC Programming Models** | |x |

## Hybrid Connections

The Azure Relay Hybrid Connections capability is a secure, open-protocol
evolution of the existing Relay features that can be implemented on any
platform and in any language. Hybrid Connections can relay WebSockets as well
as HTTP(S) requests and responses. These capabilities are compatible with
WebSocket API in common web browsers. Hybrid Connections is based on HTTP and
WebSockets.

The protocol is fully documented in the [Hybrid Connections protocol
guide](relay-hybrid-connections-protocol.md), allowing use of the Hybrid
Connections Relay with practically any Websockets library for any runtime and
language.

### Service history

Hybrid Connections supplants the former, similarly named "BizTalk Services"
feature that was built on the Azure Service Bus WCF Relay. The new Hybrid
Connections capability complements the existing WCF Relay feature and these two
service capabilities exist side-by-side in the Azure Relay service. They share
a common gateway, but are otherwise different implementations.

## WCF Relay

WCF Relay works for the full .NET Framework (NETFX) and for WCF. You initiate
the connection between your on-premises service and the relay service using a
suite of WCF "relay" bindings. Behind the scenes, the relay bindings map to new
transport binding elements designed to create WCF channel components that
integrate with Service Bus in the cloud. For more information, see [getting
started with WCF Relay](relay-wcf-dotnet-get-started.md).

## Architecture: Processing of incoming relay requests

When a client sends a request to the [Azure Relay](/azure/service-bus-relay/)
service, the Azure load balancer routes it to any of the gateway nodes. If the
request is a listening request, the gateway node creates a new relay. If the
request is a connection request to a specific relay, the gateway node forwards
the connection request to the gateway node that owns the relay. The gateway
node that owns the relay sends a rendezvous request to the listening client,
asking the listener to create a temporary channel to the gateway node that
received the connection request.

When the relay connection is established, the clients can exchange messages via
the gateway node that is used for the rendezvous.

![Processing of Incoming WCF Relay Requests](./media/relay-what-is-it/ic690645.png)

## Next steps

* [Relay FAQ](relay-faq.md)
* [Create a namespace](relay-create-namespace-portal.md)
* [Get started with .NET Websockets](relay-hybrid-connections-dotnet-get-started.md)
* [Get started with .NET HTTP Requests](relay-hybrid-connections-http-requests-dotnet-get-started.md)
* [Get started with Node Websockets](relay-hybrid-connections-node-get-started.md)
* [Get started with Node HTTP Requests](relay-hybrid-connections-http-requests-node-get-started.md)

