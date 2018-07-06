---
title: Azure Traffic Manager - FAQs | Microsoft Docs
description: This article provides answers to frequently asked questions about Traffic Manager
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jeconnoc
editor: ''

ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: kumud
---

# Traffic Manager Frequently Asked Questions (FAQ)

## Traffic Manager basics

### What IP address does Traffic Manager use?

As explained in [How Traffic Manager Works](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager works at the DNS level. It sends DNS responses to direct clients to the appropriate service endpoint. Clients then connect to the service endpoint directly, not through Traffic Manager.

Therefore, Traffic Manager does not provide an endpoint or IP address for clients to connect to. If you want static IP address for your service, that must be configured at the service, not in Traffic Manager.

### What types of traffic can be routed using Traffic Manager?
As explained in [How Traffic Manager Works](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), a Traffic Manager endpoint can be any internet facing service hosted inside or outside of Azure. Hence, Traffic Manager can route traffic that originates from the public internet to a set of endpoints that are also internet facing. If you have endpoints that are inside a private network (for example, an internal version of [Azure Load Balancer](../load-balancer/load-balancer-overview.md#internalloadbalancer)) or have users making DNS requests from such internal networks, Traffic Manager cannot be used for those traffic.


### Does Traffic Manager support 'sticky' sessions?

As explained in [How Traffic Manager Works](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager works at the DNS level. It uses DNS responses to direct clients to the appropriate service endpoint. Clients connect to the service endpoint directly, not through Traffic Manager. Therefore, Traffic Manager does not see the HTTP traffic between the client and the server.

Additionally, the source IP address of the DNS query received by Traffic Manager belongs to the recursive DNS service, not the client. Therefore, Traffic Manager has no way to track individual clients and cannot implement 'sticky' sessions. This limitation is common to all DNS-based traffic management systems and is not specific to Traffic Manager.

### Why am I seeing an HTTP error when using Traffic Manager?

As explained in [How Traffic Manager Works](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager works at the DNS level. It uses DNS responses to direct clients to the appropriate service endpoint. Clients then connect to the service endpoint directly, not through Traffic Manager. Traffic Manager does not see HTTP traffic between client and server. Therefore, any HTTP error you see must be coming from your application. For the client to connect to the application, all DNS resolution steps are complete. That includes any interaction that Traffic Manager has on the application traffic flow.

Further investigation should therefore focus on the application.

The HTTP host header sent from the client's browser is the most common source of problems. Make sure that the application is configured to accept the correct host header for the domain name you are using. For endpoints using the Azure App Service, see [configuring a custom domain name for a web app in Azure App Service using Traffic Manager](../app-service/web-sites-traffic-manager-custom-domain-name.md).

### What is the performance impact of using Traffic Manager?

As explained in [How Traffic Manager Works](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager works at the DNS level. Since clients connect to your service endpoints directly, there is no performance impact incurred when using Traffic Manager once the connection is established.

Since Traffic Manager integrates with applications at the DNS level, it does require an additional DNS lookup to be inserted into the DNS resolution chain. The impact of Traffic Manager on DNS resolution time is minimal. Traffic Manager uses a global network of name servers, and uses [anycast](https://en.wikipedia.org/wiki/Anycast) networking to ensure DNS queries are always routed to the closest available name server. In addition, caching of DNS responses means that the additional DNS latency incurred by using Traffic Manager applies only to a fraction of sessions.

The Performance method routes traffic to the closest available endpoint. The net result is that the overall performance impact associated with this method should be minimal. Any increase in DNS latency should be offset by lower network latency to the endpoint.

### What application protocols can I use with Traffic Manager?

As explained in [How Traffic Manager Works](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager works at the DNS level. Once the DNS lookup is complete, clients connect to the application endpoint directly, not through Traffic Manager. Therefore, the connection can use any application protocol. 
 If you select TCP as the monitoring protocol, Traffic Manager's endpoint health monitoring can be done without using any application protocols. If you choose to have the health verified using an application protocol, the endpoint needs to be able to respond to either HTTP or HTTPS GET requests.

### Can I use Traffic Manager with a 'naked' domain name?

No. The DNS standards do not permit CNAMEs to co-exist with other DNS records of the same name. The apex (or root) of a DNS zone always contains two pre-existing DNS records; the SOA and the authoritative NS records. This means a CNAME record cannot be created at the zone apex without violating the DNS standards.

Traffic Manager requires a DNS CNAME record to map the vanity DNS name. For example, you map www.contoso.com to the Traffic Manager profile DNS name contoso.trafficmanager.net. Additionally, the Traffic Manager profile returns a second DNS CNAME to indicate which endpoint the client should connect to.

To work around this issue, we recommend using an HTTP redirect to direct traffic from the naked domain name to a different URL, which can then use Traffic Manager. For example, the naked domain 'contoso.com' can redirect users to the CNAME 'www.contoso.com' that points to the Traffic Manager DNS name.

Full support for naked domains in Traffic Manager is tracked in our feature backlog. You can register your support for this feature request by [voting for it on our community feedback site](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### Does Traffic Manager consider the client subnet address when handling DNS queries? 
Yes, in addition to the source IP address of the DNS query it receives (which usually is the IP address of the DNS resolver), when performing lookups for Geographic and Performance routing methods, traffic manager also considers the client subnet address if it is included in the query by the resolver making the request on behalf of the end user.  
Specifically, [RFC 7871 – Client Subnet in DNS Queries](https://tools.ietf.org/html/rfc7871) that provides an [Extension Mechanism for DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) which can pass on the client subnet address from resolvers that support it.

### What is DNS TTL and how does it impact my users?

When a DNS query lands on Traffic Manager, it sets a value in the response called time-to-live (TTL). This value, whose unit is in seconds, indicates to DNS resolvers downstream on how long to cache this response. While DNS resolvers are not guaranteed to cache this result, caching it enables them to respond to any subsequent queries off the cache instead of going to Traffic Manager DNS servers. This impacts the responses as follows:
- a higher TTL reduces the number of queries that land on the Traffic Manager DNS servers, which can reduce the cost for a customer since number of queries served is a billable usage.
- a higher TTL can potentially reduce the time it takes to do a DNS lookup.
- a higher TTL also means that your data does not reflect the latest health information that Traffic Manager has obtained through its probing agents.

### How high or low can I set the TTL for Traffic Manager responses?

You can set, at a per profile level, the DNS TTL to be as low as 0 seconds and as high as 2,147,483,647 seconds (the maximum range compliant with [RFC-1035](https://www.ietf.org/rfc/rfc1035.txt )). A TTL of 0 means that downstream DNS resolvers do not cache query responses and all queries are expected to reach the Traffic Manager DNS servers for resolution.

### How can I understand the volume of queries coming to my profile? 
One of the metrics provided by Traffic Manager is the number of query responded by a profile. You can get this information at a profile level aggregation or you can split it up further to see the volume of queries where specific endpoints were returned. In addition, you can setup alerts to notify you if the query response volume crosses the conditions you have set. For more details, [Traffic Manager metrics and alerts](traffic-manager-metrics-alerts.md).

## Traffic Manager Geographic traffic routing method

### What are some use cases where geographic routing is useful? 
Geographic routing type can be used in any scenario where an Azure customer needs to distinguish their users based on geographic regions. For example, using the Geographic traffic routing method, you can give users from specific regions a different user experience than those from other regions. Another example is complying with local data sovereignty mandates that require that users from a specific region be served only by endpoints in that region.

### How do I decide if I should use Performance routing method or Geographic routing method? 
The key difference between these two popular routing methods is that in Performance routing method your primary goal is to send traffic to the endpoint that can provide the lowest latency to the caller, whereas, in Geographic routing the primary goal is to enforce a geo fence for your callers so that you can deliberately route them to a specific endpoint. The overlap happens since there is a correlation between geographical closeness and lower latency, although this is not always true. There might be an endpoint in a different geography that can provide a better latency experience for the caller and in that case Performance routing will send the user to that endpoint but Geographic routing will always send them to the endpoint you have mapped for their geographic region. To further make it clear, consider the following example - with Geographic routing you can make uncommon mappings such as send all traffic from Asia to endpoints in the US and all US traffic to endpoints in Asia. In that case, Geographic routing will deliberately do exactly what you have configured it to do and performance optimization is not a consideration. 
>[!NOTE]
>There may be scenarios where you might need both performance and geographic routing capabilities, for these scenarios nested profiles can be great choice. For example, you can set up a parent profile with geographic routing where you send all traffic from North America to a nested profile that has endpoints in the US and use performance routing to send those traffic to the best endpoint within that set. 

### What are the regions that are supported by Traffic Manager for geographic routing? 
The country/region hierarchy that is used by Traffic Manager can be found [here](traffic-manager-geographic-regions.md). While this page is kept up-to-date with any changes, you can also programmatically retrieve the same information by using the [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/). 

### How does traffic manager determine where a user is querying from? 
Traffic Manager looks at the source IP of the query (this most likely is a local DNS resolver doing the querying on behalf of the user) and uses an internal IP to region map to determine the location. This map is updated on an ongoing basis to account for changes in the internet. 

### Is it guaranteed that Traffic Manager can correctly determine the exact geographic location of the user in every case?
No, Traffic Manager cannot guarantee that the geographic region we infer from the source IP address of a DNS query will always correspond to the user’s location due to the following reasons: 

- First, as described in the previous FAQ, the source IP address we see is that of a DNS resolver doing the lookup on behalf of the user. While the geographic location of the DNS resolver is a good proxy for the geographic location of the user, it can also be different depending upon the footprint of the DNS resolver service and the specific DNS resolver service a customer has chosen to use. 
As an example, a customer located in Malaysia could specify in their device’s settings use a DNS resolver service whose DNS server in Singapore might get picked to handle the query resolutions for that user/device. In that case, Traffic Manager can only see the resolver’s IP address that corresponds to the Singapore location. Also, see the earlier FAQ regarding client subnet address support on this page.

- Second, Traffic Manager uses an internal map to do the IP address to geographic region translation. While this map is validated and updated on an ongoing basis to increase its accuracy and account for the evolving nature of the internet, there is still the possibility that our information is not an exact representation of the geographic location of all the IP addresses.


###  Does an endpoint need to be physically located in the same region as the one it is configured with for geographic routing? 
No, the location of the endpoint imposes no restrictions on which regions can be mapped to it. For example, an endpoint in US-Central Azure region can have all users from India directed to it.

### Can I assign geographic regions to endpoints in a profile that is not configured to do geographic routing? 

Yes, if the routing method of a profile is not geographic, you can use the [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) to assign geographic regions to endpoints in that profile. In the case of non-geographic routing type profiles, this configuration is ignored. If you change such a profile to geographic routing type at a later time, Traffic Manager can use those mappings.


### Why am I getting an error when I try to change the routing method of an existing profile to Geographic?

All the endpoints under a profile with geographic routing need to have at least one region mapped to it. To convert an existing profile to geographic routing type, you first need to associate geographic regions to all its endpoints using the [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) before changing the routing type to geographic. If using portal, first delete the endpoints, change the routing method of the profile to geographic and then add the endpoints along with their geographic region mapping. 


###  Why is it strongly recommended that customers create nested profiles instead of endpoints under a profile with geographic routing enabled? 

A region can be assigned to only one endpoint within a profile if its using geographic routing type. If that endpoint is not a nested type with a child profile attached to it, if that endpoint going unhealthy, Traffic Manager continues to send traffic to it since the alternative of not sending any traffic isn’t any better. Traffic Manager does not failover to another endpoint, even when the region assigned is a “parent” of the region assigned to the endpoint that went unhealthy (for example, if an endpoint that has region Spain goes unhealthy, we do not failover to another endpoint that has the region Europe assigned to it). This is done to ensure that Traffic Manager respects the geographic boundaries that a customer has setup in their profile. To get the benefit of failing over to another endpoint when an endpoint goes unhealthy, it is recommended that geographic regions be assigned to nested profiles with multiple endpoints within it instead of individual endpoints. In this way, if an endpoint in the nested child profile fails, traffic can failover to another endpoint inside the same nested child profile.

### Are there any restrictions on the API version that supports this routing type?

Yes, only API version 2017-03-01 and newer supports the Geographic routing type. Any older API versions cannot be used to created profiles of Geographic routing type or assign geographic regions to endpoints. If an older API version is used to retrieve profiles from an Azure subscription, any profile of Geographic routing type is not returned. In addition, when using older API versions, any profile returned that has endpoints with a geographic region assignment, does not have its geographic region assignment shown.

## Real User Measurements

### What are the benefits of using Real User Measurements?
When you use performance routing method, Traffic Manager picks the best Azure region for your end user to connect to by inspecting the source IP and EDNS Client Subnet (if passed in) and checking it against the network latency intelligence the service maintains. Real User Measurements enhances this for your end user base by having their experience contribute to this latency table in addition to ensuring that this table adequately spans the end user networks from where your end users connect to Azure. This leads to an increased accuracy in the routing of your end users.

### Can I use Real User Measurements with non-Azure regions?
Real User Measurements measures and reports on only the latency to reach Azure regions. If you are using performance-based routing with endpoints hosted in non-Azure regions, you can still benefit from this feature by having increased latency information about the representative Azure region you had selected to be associated with this endpoint.

### Which routing method benefits from Real User Measurements?
The additional information gained through Real User Measurements are applicable only for profiles that use the performance routing method. Note that the Real User Measurements link is available from all the profiles when you view it through the Azure portal.

### Do I need to enable Real User Measurements each profile separately?
No, you only need to enable it once per subscription and all the latency information measured and reported are available to all profiles.

### How do I turn off Real User Measurements for my subscription?
You can stop accruing charges related to Real User Measurements when you stop collecting and sending back latency measurements from your client application. For example, when measurement JavaScript embedded in web pages, you can stop using this feature by removing the JavaScript or by turning off its invocation when the page is rendered.

You can also turn off Real User Measurements by deleting your key. Once you delete the key, any measurements sent to Traffic Manager with that key are discarded.

### Can I use Real User Measurements with client applications other than web pages?
Yes, Real User Measurements is designed to ingest data collected through different type of end user clients. This FAQ will be updated as new types of client applications get supported.

### How many measurements are made each time my Real User Measurements enabled web page is rendered?
When Real User Measurements is used with the measurement JavaScript provided, each page rendering results in six measurements being taken. These are then reported back to the Traffic Manager service. Note that you are charged for this feature based on the number of measurements reported to Traffic Manager service. For example, if the user navigates away from your webpage while the measurements are being taken but before it was reported, those measurements are not considered for billing purposes.

### Is there a delay before Real User Measurements script runs in my webpage?
No, there is no programmed delay before the script is invoked.

### Can I use configure Real User Measurements with only the Azure regions I want to measure?
No, each time it is invoked, the Real User Measurements script measures a set of six Azure regions as determined by the service. This set changes between different invocations and when a large number of such invocations happen, the measurement coverage spans across different Azure regions.

### Can I limit the number of measurements made to a specific number?
The measurement JavaScript is embedded within your webpage and you are in complete control over when to start and stop using it. As long as the Traffic Manager service receives a request for a list of Azure regions to be measured, a set of regions are returned.

### Can I see the measurements taken by my client application as part of Real User Measurements?
Since the measurement logic is run from your client application, you are in full control of what happens including seeing the latency measurements. Traffic Manager does not report an aggregate view of the measurements received under the key linked to your subscription.

### Can I modify the measurement script provided by Traffic Manager?
While you are in control of what is embedded on your web page, we strongly discourage you from making any changes to the measurement script to ensure that it measures and reports the latencies correctly.

### Will it be possible for others to see the key I use with Real User Measurements?
When you embed the measurement script to a web page it will be possible for others to see the script and your Real User Measurements (RUM) key. But it is important to know that this key is different from your subscription id and is generated by Traffic Manager to be used only for this purpose. Knowing your RUM key will not compromise your Azure account safety.

### Can others abuse my RUM key?
While it is possible for others to use your key to send wrong information to Azure please note that a few wrong measurements will not change the routing since it is taken into account along with all the other measurements we receive. If you need to change your keys, you can re-generate the key at which point the old key becomes discarded.

###  Do I need to put the measurement JavaScript in all my web pages?
Real User Measurements delivers more value as the number of measurements increase. Having said that, it is your decision as to whether you need to put it in all your web pages or a select few. Our recommendation is to start by putting it in your most visited page where a user is expected to stay on that page five seconds or more.

### Can information about my end users be identified by Traffic Manager if I use Real User Measurements?
When the provided measurement JavaScript is used, Traffic Manager will have visibility into the client IP address of the end user and the source IP address of the local DNS resolver they use. Traffic Manager uses the client IP address only after having it truncated to not be able to identify the specific end user who sent the measurements. 

### Does the webpage measuring Real User Measurements need to be using Traffic Manager for routing?
No, it doesn’t need to use Traffic Manager. The routing side of Traffic Manager operates separately from the Real User Measurement part and although it is a great idea to have them both in the same web property, they don’t need to be.

### Do I need to host any service on Azure regions to use with Real User Measurements?
No, you don’t need to host any server side component on Azure for Real User Measurements to work. The single pixel image downloaded by the measurement JavaScript and the service running it in different Azure regions is hosted and managed by Azure. 

### Will my Azure bandwidth usage increase when I use Real User Measurements?
As mentioned in the previous answer, the server-side components of Real User Measurements are owned and managed by Azure. This means your Azure bandwidth usage will not increase because you use Real User Measurements. Please note that, this does not include any bandwidth usage outside of what Azure charges. We minimize the bandwidth used by downloading only a single pixel image to measurement the latency to an Azure region. 

## Traffic View

### What does Traffic View do?
Traffic View is a feature of Traffic Manager that helps you understand more about your users and how their experience is. It uses the queries received by Traffic Manager and the network latency intelligence tables that the service maintains to provide you with the following:
- The regions from where your users are connecting to your endpoints in Azure.
- The volume of users connecting from these regions.
- The Azure regions  to which they are getting routed to.
- Their latency experience to these Azure regions.

This information is available for you to consume through geographical map overlay and tabular views in the portal in addition to being available as raw data for you to download.

### How can I benefit from using Traffic View?

Traffic View gives you the overall view of the traffic your Traffic Manager profiles receive. In particular, it can be used to understand where your user base connects from and equally importantly what their average latency experience is. You can then use this information to find areas in which you need to focus, for example, by expanding your Azure footprint to a region that can serve those users with lower latency. Another insight you can derive from using Traffic View is to see the patterns of traffic to different regions which in turn can help you make decisions on increasing or decreasing invent in those regions.

### How is Traffic View different from the Traffic Manager metrics available through Azure monitor?

Azure Monitor can be used to understand at an aggregate level the traffic received by your profile and its endpoints. It also enables you to track the health status of the endpoints by exposing the health check results. When you need to go beyond these and understand your end user’s experience connecting to Azure at a regional level, Traffic View can be used to achieve that.

### Does Traffic View use EDNS Client Subnet information?

The DNS queries served by Azure Traffic Manager do consider ECS information to increase the accuracy of the routing. But when creating the data set that shows where the users are connecting from, Traffic View is using only the IP address of the DNS resolver.

### How many days of data does Traffic View use?

Traffic View creates its output by processing the data from the seven days preceding the day before when it is viewed by you. This is a moving window and the latest data will be used each time you visit.

### How does Traffic View handle external endpoints?

When you use external endpoints hosted outside Azure regions in a Traffic Manager profile you can choose to have it mapped to an Azure region which is a proxy for its latency characteristics (this is in fact needed if you use performance routing method). If it has this Azure region mapping, that Azure region’s latency metrics will be used when creating the Traffic View output. If no Azure region is specified, the latency information will be empty in the data for those external endpoints.

### Do I need to enable Traffic View for each profile in my subscription?

During the preview period, Traffic View was enabled at a subscription level. As part of the improvements we made before the general availability, you can now enable Traffic View at a profile level, allowing you to have more granular enabling of this feature. By default, Traffic View will be disabled for a profile.

>[!NOTE]
>If you enabled Traffic View at a subscription level during the preview time, you now need to re-enable it for each of the profile under that subscription.
 
### How can I turn off Traffic View? 
You can turn off Traffic View for any profile using the Portal or REST API. 

### How does Traffic View billing work?

Traffic View pricing is based on the number of data points used to create the output. Currently, the only data type supported is the queries your profile receives. In addition, you are only billed for the processing that was done when you have Traffic View enabled. This means that, if you enable Traffic View for some time period in a month and turn it off during other times, only the data points processed while you had the feature enabled count towards your bill.

## Traffic Manager endpoints

### Can I use Traffic Manager with endpoints from multiple subscriptions?

Using endpoints from multiple subscriptions is not possible with Azure Web Apps. Azure Web Apps requires that any custom domain name used with Web Apps is only used within a single subscription. It is not possible to use Web Apps from multiple subscriptions with the same domain name.

For other endpoint types, it is possible to use Traffic Manager with endpoints from more than one subscription. In Resource Manager, endpoints from any subscription can be added to Traffic Manager, as long as the person configuring the Traffic Manager profile has read access to the endpoint. These permissions can be granted using [Azure Resource Manager role-based access control (RBAC)](../role-based-access-control/role-assignments-portal.md).


### Can I use Traffic Manager with Cloud Service 'Staging' slots?

Yes. Cloud Service 'staging' slots can be configured in Traffic Manager as External endpoints. Health checks are still be charged at the Azure Endpoints rate. Because the External endpoint type is in use, changes to the underlying service are not picked up automatically. With external endpoints, Traffic Manager cannot detect when the Cloud Service is stopped or deleted. Therefore, the Traffic Manager continues billing for health checks until the endpoint is disabled or deleted.

### Does Traffic Manager support IPv6 endpoints?

Traffic Manager does not currently provide IPv6-addressible name servers. However, Traffic Manager can still be used by IPv6 clients connecting to IPv6 endpoints. A client does not make DNS requests directly to Traffic Manager. Instead, the client uses a recursive DNS service. An IPv6-only client sends requests to the recursive DNS service via IPv6. Then the recursive service should be able to contact the Traffic Manager name servers using IPv4.

Traffic Manager responds with the DNS name of the endpoint. To support an IPv6 endpoint, a DNS AAAA record pointing the endpoint DNS name to the IPv6 address must exist. Traffic Manager health checks only support IPv4 addresses. The service needs to expose an IPv4 endpoint on the same DNS name.

### Can I use Traffic Manager with more than one Web App in the same region?

Typically, Traffic Manager is used to direct traffic to applications deployed in different regions. However, it can also be used where an application has more than one deployment in the same region. The Traffic Manager Azure endpoints do not permit more than one Web App endpoint from the same Azure region to be added to the same Traffic Manager profile.

### How do I move my Traffic Manager profile’s Azure endpoints to a different resource group?

Azure endpoints that are associated with a Traffic Manager profile are tracked using their resource IDs. When an Azure resource that is being used as an endpoint (for example,  Public IP, Classic Cloud Service, WebApp, or another Traffic Manager profile used in a nested manner) is moved to a different resource group, its resource ID changes. In this scenario, currently, you must update the Traffic Manager profile by first deleting and then adding back the endpoints to the profile. 

##  Traffic Manager endpoint monitoring

### Is Traffic Manager resilient to Azure region failures?

Traffic Manager is a key component of the delivery of highly available applications in Azure.
To deliver high availability, Traffic Manager must have an exceptionally high level of availability and be resilient to regional failure.

By design, Traffic Manager components are resilient to a complete failure of any Azure region. This resilience applies to all Traffic Manager components: the DNS name servers, the API, the storage layer, and the endpoint monitoring service.

In the unlikely event of an outage of an entire Azure region, Traffic Manager is expected to continue to function normally. Applications deployed in multiple Azure regions can rely on Traffic Manager to direct traffic to an available instance of their application.

### How does the choice of resource group location affect Traffic Manager?

Traffic Manager is a single, global service. It is not regional. The choice of resource group location makes no difference to Traffic Manager profiles deployed in that resource group.

Azure Resource Manager requires all resource groups to specify a location, which determines the default location for resources deployed in that resource group. When you create a Traffic Manager profile, it is created in a resource group. All Traffic Manager profiles use **global** as their location, overriding the resource group default.

### How do I determine the current health of each endpoint?

The current monitoring status of each endpoint, in addition to the overall profile, is displayed in the Azure portal. This information also is available via the Traffic Monitor [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell cmdlets](https://msdn.microsoft.com/library/mt125941.aspx), and [cross-platform Azure CLI](../cli-install-nodejs.md).

You can also use Azure Monitor to track the health of your endpoints and see a visual representation of them. For more about using Azure Monitor, see the [Azure Monitoring documentation](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

### Can I monitor HTTPS endpoints?

Yes. Traffic Manager supports probing over HTTPS. Configure **HTTPS** as the protocol in the monitoring configuration.

Traffic manager cannot provide any certificate validation, including:

* Server-side certificates are not validated
* SNI server-side certificates are not supported
* Client certificates are not supported

### I stopped an Azure cloud service / web application endpoint in my Traffic Manager profile but I am not receiving any traffic even after I restarted it. How can I fix this?

When an Azure cloud service / web application endpoint is stopped Traffic Manager stops checking its health and restarts the health checks only after it detects that the endpoint has restarted. To prevent this delay, disable and then reenable that endpoint in the Traffic Manager profile after you restart the endpoint.   

### Can I use Traffic Manager even if my application does not have support for HTTP or HTTPS?

Yes. You can specify TCP as the monitoring protocol and Traffic Manager can initiate a TCP connection and wait for a response from the endpoint. If the endpoint replies to the connection request with a response to establish the connection, within the timeout period, then that endpoint is marked as healthy.

### What specific responses are required from the endpoint when using TCP monitoring?

When TCP monitoring is used, Traffic Manager starts a three-way TCP handshake by sending a SYN request to endpoint at the specified port. It then waits for a period of time (as specified in the timeout settings) for a response from the endpoint. If the endpoint responds to the SYN request with a SYN-ACK response within the timeout period specified in the monitoring settings, then that endpoint is considered healthy. If the SYN-ACK response is received, the Traffic Manager resets the connection by responding back with a RST.

### How fast does Traffic Manager move my users away from an unhealthy endpoint?

Traffic Manager provides multiple settings that can help you to control the failover behavior of your Traffic Manager profile as follows:
- you can specify that the Traffic Manager probes the endpoints more frequently by setting the Probing Interval at 10 seconds. This ensures that any endpoint going unhealthy can be detected as soon as possible. 
- you can specify how long to wait before a health check request times out (minimum time out value is 5 sec).
- you can specify how many failures can occur before the endpoint is marked as unhealthy. This value can be low as 0, in which case the endpoint is marked unhealthy as soon as it fails the first health check. However, using the minimum value of 0 for the tolerated number of failures can lead to endpoints being taken out of rotation due to any transient issues that may occur at the time of probing.
- you can specify the time-to-live (TTL) for the DNS response to be as low as 0. Doing so means that DNS resolvers cannot cache the response and each new query gets a response that incorporates the most up-to-date health information that the Traffic Manager has.

By using these settings, Traffic Manager can provide failovers under 10 seconds after an endpoint goes unhealthy and a DNS query is made against the corresponding profile.

### How can I specify different monitoring settings for different endpoints in a profile?

Traffic Manager monitoring settings are at a per profile level. If you need to use a different monitoring setting for only one endpoint, it can be done by having that endpoint as a [nested profile](traffic-manager-nested-profiles.md) whose monitoring settings are different from the parent profile.

### What host header do endpoint health checks use?

Traffic Manager uses host headers in HTTP and HTTPS health checks. The host header used by Traffic Manager is the name of the endpoint target configured in the profile. The value used in the host header cannot be specified separately from the target property.

### What are the IP addresses from which the health checks originate?

Click [here](https://azuretrafficmanagerdata.blob.core.windows.net/probes/azure/probe-ip-ranges.json) to view the JSON file that lists the IP addresses from which Traffic Manager health checks can originate. Review the IPs listed in the JSON file to ensure that incoming connections from these IP addresses are allowed at the endpoints to check its health status.

### How many health checks to my endpoint can I expect from Traffic Manager?

The number of Traffic Manager health checks reaching your endpoint depends on the following:
- the value that you have set for the monitoring interval (smaller interval means more requests landing on your endpoint in any given time period).
- the number of locations from where the health checks originate (the IP addresses from where you can expect these checks is listed in the preceding FAQ).

### How can I get notified if one of my endpoints goes down? 
One of the metrics provided by Traffic Manager is the health status of endpoints in a profile. You can see this as an aggregate of all endpoints inside a profile (for example, 75% of your endpoints are healthy), or, at a per endpoint level. Traffic Manager metrics are exposed through Azure Monitor and you can use its [alerting capabilities](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) to get notifications when there is a change in the health status of your endpoint. For more details, see [Traffic Manager metrics and alerts](traffic-manager-metrics-alerts.md).  

## Traffic Manager nested profiles

### How do I configure nested profiles?

Nested Traffic Manager profiles can be configured using both the Azure Resource Manager and the classic Azure REST APIs, Azure PowerShell cmdlets and cross-platform Azure CLI commands. They are also supported via the new Azure portal.

### How many layers of nesting does Traffic Manger support?

You can nest profiles up to 10 levels deep. 'Loops' are not permitted.

### Can I mix other endpoint types with nested child profiles, in the same Traffic Manager profile?

Yes. There are no restrictions on how you combine endpoints of different types within a profile.

### How does the billing model apply for Nested profiles?

There is no negative pricing impact of using nested profiles.

Traffic Manager billing has two components: endpoint health checks and millions of DNS queries

* Endpoint health checks: There is no charge for a child profile when configured as an endpoint in a parent profile. Monitoring of the endpoints in the child profile is billed in the usual way.
* DNS queries: Each query is only counted once. A query against a parent profile that returns an endpoint from a child profile is counted against the parent profile only.

For full details, see the [Traffic Manager pricing page](https://azure.microsoft.com/pricing/details/traffic-manager/).

### Is there a performance impact for nested profiles?

No. There is no performance impact incurred when using nested profiles.

The Traffic Manager name servers traverse the profile hierarchy internally when processing each DNS query. A DNS query to a parent profile can receive a DNS response with an endpoint from a child profile. A single CNAME record is used whether you are using a single profile or nested profiles. There is no need to create a CNAME record for each profile in the hierarchy.

### How does Traffic Manager compute the health of a nested endpoint in a parent profile?

The parent profile doesn't perform health checks on the child directly. Instead, the health of the child profile's endpoints are used to calculate the overall health of the child profile. This information is propagated up the nested profile hierarchy to determine the health of the nested endpoint. The parent profile uses this aggregated health to determine whether the traffic can be directed to the child.

The following table describes the behavior of Traffic Manager health checks for a nested endpoint.

| Child Profile Monitor status | Parent Endpoint Monitor status | Notes |
| --- | --- | --- |
| Disabled. The child profile has been disabled. |Stopped |The parent endpoint state is Stopped, not Disabled. The Disabled state is reserved for indicating that you have disabled the endpoint in the parent profile. |
| Degraded. At least one child profile endpoint is in a Degraded state. |Online: the number of Online endpoints in the child profile is at least the value of MinChildEndpoints.<BR>CheckingEndpoint: the number of Online plus CheckingEndpoint endpoints in the child profile is at least the value of MinChildEndpoints.<BR>Degraded: otherwise. |Traffic is routed to an endpoint of status CheckingEndpoint. If MinChildEndpoints is set too high, the endpoint is always degraded. |
| Online. At least one child profile endpoint is an Online state. No endpoint is in the Degraded state. |See above. | |
| CheckingEndpoints. At least one child profile endpoint is 'CheckingEndpoint'. No endpoints are 'Online' or 'Degraded' |Same as above. | |
| Inactive. All child profile endpoints are either Disabled or Stopped, or this profile has no endpoints. |Stopped | |

## Next steps:
- Learn more about Traffic Manager [endpoint monitoring and automatic failover](../traffic-manager/traffic-manager-monitoring.md).
- Learn more about Traffic Manager [traffic routing methods](../traffic-manager/traffic-manager-routing-methods.md).
