# Lab 1.1 Findings
## My VM Details
- Provider: Amazon Web Services (AWS)
- Region: eu-north-1
- OS: Ubuntu 26.04 LTS

## Experiment Results
### Ping to 8.8.8.8
- Average RTT: "Not Available"
- TTL value observed: "Not Available"
- What does TTL tell us about the path? <br>
  TTL could not be determined because no ICMP Echo Reply packets were received from the destination. TTL normally indicates the number of router hops a packet has traversed before reaching the destination.

### Traceroute to google.com
- Number of hops: 30
- Any * * * hops? At which hop number? <br>
  Yes at Hop 2

### DNS Comparison
- Result from default DNS: <br>
  Name:   google.com <br>
  Address: 216.58.207.110 <br>
  Name:   google.com <br>
  Address: 2a00:1450:400f:806::200e <br>

- Result from 1.1.1.1: <br>
  No server Could be reached <br>
  
- Are they different? Why might they differ? <br>
  Yes they are different. <br>
  DNS resolution using the system-configured resolver was successful. However, direct queries to the external DNS server (1.1.1.1) failed with "No servers could be reached." This indicates that the host can communicate with its configured DNS infrastructure, but outbound DNS traffic to the specified external resolver is blocked or unreachable due to firewall rules, network policies, routing issues, or VPN restrictions.

### google.com TLS Certificate
- Issuer: C=US, O=Google Trust Services, CN=WR2
- Expiry date: Aug 17 08:39:10 2026 GMT
- TLS version used: v1.3
