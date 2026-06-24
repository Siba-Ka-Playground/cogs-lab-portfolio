
# Lab 3.1 Findings: Uptime Kuma vs. Site24x7

## 1. Monitor Type Mapping
Here is the mapping between Uptime Kuma monitor types and their equivalent Site24x7 monitor types:

* **Uptime Kuma: HTTP(S)** -> **Site24x7: Website (HTTP/HTTPS) or REST API Monitor**
  * *Purpose:* Monitors web servers or API endpoints for HTTP 200 OK responses and measures response times.
* **Uptime Kuma: TCP Port** -> **Site24x7: Port (TCP) Monitor**
  * *Purpose:* Checks if a specific network port (e.g., 22 for SSH, 3306 for MySQL, 443 for HTTPS) is open and actively accepting network connections.
* **Uptime Kuma: Ping** -> **Site24x7: Ping Monitor**
  * *Purpose:* Sends ICMP Echo Requests to verify basic network-level reachability and measure packet loss or latency.

## 2. Monitoring an InstaSafe Gateway
**Question:** Which type would you use to monitor an InstaSafe Gateway?

**Answer:** To effectively monitor an InstaSafe Gateway, the most appropriate choice is a combination, but primarily a **TCP Port Monitor**.

* **Primary - TCP Port Monitor:** InstaSafe Gateways operate by listening on specific ports to facilitate secure Zero Trust Network Access (ZTNA) tunnels. Monitoring the specific TCP port configured for the gateway ensures that the actual gateway service/daemon is actively running and accepting secure connections, rather than just checking if the server is turned on.
* **Secondary - Ping Monitor:** Used as a baseline to ensure the underlying server hosting the gateway is online and accessible over the network.
* **Optional - HTTP(S):** If the InstaSafe Gateway exposes a web-based administrative console or a specific HTTPS health check endpoint, an HTTP(S) monitor could also be used to verify the application layer is responding correctly.
