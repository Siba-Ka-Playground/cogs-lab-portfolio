# P1 Lab Submission: Nginx Gateway Unreachable

## 📝 Post-Incident Review (PIR)

### 1. Executive Summary
On 24-06-2026, an outage occurred rendering the Nginx Gateway completely unreachable, disconnecting all users. The incident was detected immediately via Uptime Kuma, investigated, and resolved within 30 minutes by restarting the web server daemon. 

### 2. Timeline (Timezone: .....)
* **T+0 [Time]:** Uptime Kuma monitoring detected the Nginx service as offline.
* **T+2 [Time]:** P1 Incident ticket created in GitHub.
* **T+3 [Time]:** Initial customer acknowledgement dispatched.
* **T+5 [Time]:** Uptime Kuma logs reviewed for exact failure timestamp.
* **T+8 [Time]:** Prometheus metrics queried; no CPU or memory spikes detected prior to failure.
* **T+20 [Time]:** Nginx service manually restarted via systemctl.
* **T+22 [Time]:** Uptime Kuma confirmed service recovery (Green).
* **T+30 [Time]:** Resolution notice posted and ticket closed.

### 3. Root Cause Analysis
The outage was caused by the `nginx` system service being manually stopped (`sudo systemctl stop nginx`), simulating a catastrophic daemon crash. Because there was no automated recovery mechanism configured for the service, the gateway remained offline until manual intervention occurred.

### 4. Impact
* **Severity:** P1 - Critical
* **Scope:** All users utilizing the ZTNA product.
* **Duration of Outage:** ~22 minutes.

### 5. Resolution
The responding engineer established an SSH session to the affected server and issued the `sudo systemctl start nginx` command. Service connectivity was immediately restored and verified via Uptime Kuma.

### 6. Action Items
1.  Configure `systemd` to automatically restart the Nginx service on failure (`Restart=on-failure` in the service file).
2.  Implement alert routing to page the on-call engineer immediately upon Uptime Kuma detecting a state change, rather than relying on manual dashboard observation.

### 7. Prevention
Implementing the auto-restart configuration will ensure that if the daemon crashes unexpectedly in the future, the system will self-heal within seconds, preventing a prolonged P1 scenario.

---

## 🤝 PACE Handover Note

**To:** Incoming SRE Shift / XYZ Engineer
**From:** Sibasundar, Outgoing SRE

* **Primary (Current Status):** The Nginx Gateway is stable and operational. A P1 incident occurred earlier during this shift where the service crashed. It has been fully restored and the ticket is closed.
* **Alternate (Pending Actions):** Please monitor the Uptime Kuma dashboard over the next 4 hours to ensure the Nginx service remains stable. No immediate technical action is required.
* **Contingency (Known Issues):** As noted in the PIR, the service does not currently have auto-restart configured. If it crashes again, you will need to manually SSH into the VM and run `sudo systemctl start nginx`. 
* **Emergency (Escalation):** If the service fails to start or Prometheus shows abnormal CPU/Memory usage, escalate immediately to the Tier 3 Platform team.

**Incoming Engineer Confirmation:** [ ] Acknowledged by incoming shift engineer.
