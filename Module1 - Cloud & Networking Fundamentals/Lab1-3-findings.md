# ZTNA Component Mapping
## WireGuard to ZTNA Component Mapping

This document maps the components of a manual WireGuard VPN tunnel to the architecture of an InstaSafe ZTNA deployment.

| WireGuard Component | ZTNA Equivalent | Functionality Mapping |
| :--- | :--- | :--- |
| **WireGuard Client (VM2)** | **InstaSafe Agent** | The endpoint software installed on the user device that initiates the encrypted tunnel to the gateway. |
| **WireGuard Server (VM1)** | **InstaSafe Gateway** | The point of ingress that terminates the encrypted tunnel and enforces access policies before forwarding traffic to internal resources. |
| **WireGuard Tunnel (wg0)** | **ZTNA Secure Tunnel** | An encrypted, authenticated "dark" connection that provides secure transit, ensuring traffic is invisible to unauthorized parties. |
| **Public/Private Keys** | **Identity Certificates** | Used for mutual authentication. Just as keys identify the peers in WireGuard, certificates identify the Agent and Gateway to the ZTNA Controller. |
| **Manual Configuration** | **ZTNA Controller** | In our lab, we manually configured the peers. In a ZTNA environment, a central Controller automates this key exchange and policy distribution. |

## Conclusion
This lab demonstrates that ZTNA is essentially an evolution of point-to-point encrypted tunnels. By replacing manual key management with automated certificate issuance and wrapping this tunnel in an identity-aware policy engine, we move from a simple VPN to a Zero Trust architecture.
