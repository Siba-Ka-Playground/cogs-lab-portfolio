# Capstone Project: Mini-ZTNA Architecture

## 1. Architecture Diagram

Below is the logical flow of the simplified Zero Trust Network Access (ZTNA) model built for this capstone, demonstrating the separation of the control plane (Identity) and the data plane (Gateway).

```text
                                +-------------------------------------------+
                                |               SECURE ZONE                 |
                                |                                           |
+---------------+               |  +---------------+      +--------------+  |
|               |               |  |               |      |              |  |
|  User Laptop  |   [Encrypted  |  | VM2 (Gateway) |      | VM1 (Server) |  |
|  (WG Client)  |====Tunnel]======>| Nginx Proxy   |----->| Keycloak IdP |  |
|               |  10.0.0.0/24  |  | (10.0.0.1)    |      | App (:8080)  |  |
+---------------+               |  |               |      |              |  |
      |                         |  +---------------+      +--------------+  |
      |                         |          |                      |         |
      |                         +-------------------------------------------+
      |                                    |                      |
      v                                    v                      v
+---------------------------------------------------------------------------+
|                          MONITORING INFRASTRUCTURE                        |
|                     Uptime Kuma (Health) / Prometheus (Metrics)           |
+---------------------------------------------------------------------------+
```

## How does this mini-ZTNA map to the real InstaSafe product?

The architecture constructed in this capstone serves as a foundational micro-model of commercial Zero Trust Network Access (ZTNA) solutions like InstaSafe. By breaking down the traditional network perimeter, this setup enforces the core ZTNA philosophy: "never trust, always verify."

In our model, the WireGuard tunnel directly maps to the InstaSafe Agent. Rather than exposing internal services to the public internet, WireGuard establishes a lightweight, cryptographically secure transport layer from the user's laptop. It ensures that only devices with the correct cryptographic keys can even "see" the gateway, effectively cloaking the infrastructure from unauthorized external scanning.

Our VM2 running Nginx acts as the InstaSafe Gateway. In a ZTNA model, the gateway is the Policy Enforcement Point (PEP). Nginx sits at the edge of the secure zone, terminating the secure tunnel and acting as a reverse proxy. It prevents direct lateral movement; the user cannot directly ping or scan VM1. They must request a specific resource (/app), and Nginx routes that specific, isolated request.

Finally, Keycloak on VM1 represents the Identity Provider (IdP). While Nginx controls network routing, Keycloak acts as the Policy Decision Point (PDP). Before the application serves data, Keycloak ensures the user is cryptographically authenticated via SAML/OIDC, shifting the security perimeter from the network boundary directly to the user's identity.

## What is missing compared to a production deployment?

While this model successfully demonstrates identity-aware proxying, a production deployment like InstaSafe incorporates several advanced security vectors that are absent here.

First, Device Posture Assessment is missing. Our WireGuard setup checks if the user has the right cryptographic key, but it does not evaluate the health of the connecting laptop. A production ZTNA agent continuously verifies if the OS is patched, if an EDR/antivirus is running, and if the device is corporate-issued before granting access.

Second, this model lacks Continuous Trust Verification. In our setup, once a user authenticates via Keycloak, their session is trusted for its duration. Modern ZTNA implementations continuously monitor behavioral analytics and can dynamically revoke access mid-session if anomalous activity (like a sudden change in geolocation or impossible travel) is detected.

Third, our architecture lacks Micro-segmentation at scale. We are proxying a single application route (/app). Production gateways dynamically generate micro-tunnels per application, ensuring that a compromised user account only has access to a hyper-specific application layer, not the broader subnet.

Finally, a production deployment requires High Availability and Centralized Orchestration. We are using a static Nginx configuration on a single VM. Enterprise environments utilize a centralized SaaS control plane to dynamically push access policies to geographically distributed gateway clusters, coupled with comprehensive SIEM integration for deep audit logging.
