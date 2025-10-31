To deploy large-scale efficient server farms:
- front-end load-balancers requests over serves
- servers access the same backend database
![[Pasted image 20250108120048.png]]

Back in the day: web cache used to store local copy of more recently required HTTP objects and reacts as proxy server to clients' requests
1. Keep traffic local, decreasing pressure on internet transit
2. Better quality of experience for end users
3. Enables localized content

HTTP Caching : clients often cache documents. HTTP includes caching information in headers.
If not expired: use cached only
if expired: use GET condition to request to origin

Today we use SSL (encrypted transport) --> breaks the proxy method

Problems: 
- server farms do nothing about network congestion or to improve latency issues
- caching proxies serve only their clients, not all users on the internet
- content providers can't rely on existence and correct implementation of caching proxies
- accounting issues with caching proxies
- a significant fraction of HTTP objects are un-cacheable

Content Delivery Network (CDN) is an overlaid network of Caches = Content Servers = Delivery Nodes = Replicas
![[Pasted image 20250108130958.png]]
Proactive solution of placing content closer to user

- alternative way of adding caches in the internet
- website performance improvement
- bandwidth
- availability
- load balancing

Usage: 
- Distributed web hosting
- Video on demand
- scalable live streaming
- dynamic content
- conditional-access content

![[Pasted image 20250108131356.png]]

CDN vs Caching Proxies:
- Caches are used by ISPs to reduce bandwidth consumption, CDNs are used by content providers to improve quality of service to end users
- Caches are reactive, CDNs are proactive
- Caching proxies cater to their users and not to content providers, CDNs cater to the content providers and clients
- CDNs give control over the content to content providers, caching proxies do not

Content Replication in Content Delivery Networks
- The CDN company installs thousands of servers throughout Internet
- The CDN replicates customers’ content
- When the content provider updates the content, the CDN updates the servers.
![[Pasted image 20250108131534.png]]

The origin server rewrites pages to serve content via CDN

The core idea of CDN consists in replicating content on many servers

However, the practical accomplishment of this idea encompasses multiple challenges:
- How to replicate content
- Where to replicate content
- How to find replicated content
- How to choose among server replicas
- How to direct clients towards server replicas

Server Selection:
- Which server?
	- Lowest load: to balance load on servers
	- Best performance: to improve client performance 
	- Any alive node: to provide fault tolerance
	- Based on Geography? RTT? Throughput? Load?

- How to direct clients to a particular server? 
	- As part of routing: **anycast**, cluster load balancing 
	- As part of application: HTTP redirect 
	- As part of naming: DNS

-  Routing based (IP anycast)
	- Pros: Transparent to clients, circumvents many routing issues
	- Cons: Little control, complex, scalability 
- Application based (HTTP redirects)
	- Pros: Application-level, fine-grained control 
	- Cons: Additional load and RTTs, hard to cache 
- Naming based (DNS selection) 
	- Pros: Well-suitable for caching, reduce RTTs
	- Cons: Request by resolver not client, request for domain not URL, hidden load factor of resolver’s population

## DNS-based CDN
a new DNS Server Architecture
![[Pasted image 20250108134357.png]]

Host Names are used to redirect the traffic to the best replica 
- the replica selections happens when the name is translated to an IP address

DNS servers become “Content Routers” 
- they measure as many metric as possible (RTT, Server Load, Layer 3 metrics, response time, etc.) to compute a replica routing table {{DNS-names, IP-SA} → IP-DA}
- Metric measurement is not easy
- Layer 3 metrics alone are not particularly meaningful

Limitations:
- The granularity of redirection is an host name, not a URL
- Content of large web sites cannot be split into multiple caches
- It is difficult to use the same host name for static and dynamic content

Akamai approach: change URL to point to akamai replica servers
![[Pasted image 20250108135452.png]]
- http://cnn.com/a.gif --> http://a128.g.akamai.net/7/23/cnn.com/a.gif

Using multiple domain names for each client allows the CDN to further subdivide the content into groups
DNS sees only the requested domain name, but it can route requests for different domains independently

![[Pasted image 20250108135617.png]]

