# NOC Runbooks — DDoS Mitigation 🛡️

A collection of operational runbooks for common DDoS attack types handled in a live NOC environment.

Each runbook follows the same structure used in real NOC operations — from alert detection through to resolution and documentation. Written based on hands-on L3/L4 DDoS mitigation experience.


"An alert is not just a notification — it's a starting point for investigation."




📁 Runbooks


1. udp-flood-null-route.md — UDP Flood — Null Route Mitigation
2. syn-flood-mitigation.md — SYN Flood — SYN Cookie / Rate Limiting
3. icmp-flood-response.md — ICMP Flood — Rate Limiting / Block
4. ack-flood-detection.md — ACK Flood — Stateful Inspection
5. http-flood-cwaf.md — HTTP Flood — Cloud WAF Rule Enforcement
6. ddos-escalation-checklist.md — Escalation Decision Tree



🔍 Runbook Structure

Every runbook contains:


1. What it is — attack definition and how it works
2. How it looks — traffic patterns, alert signatures, tool indicators
3. Verification steps — what to check in Grafana, Kentik, Pingdom
4. Mitigation actions — step-by-step response procedure
5. Escalation criteria — when to escalate and to whom
6. Documentation — how to log the incident in Jira and Confluence



🛠️ Tools Referenced


1. Grafana — traffic visualization and alert monitoring
2. Kentik — flow analysis and DDoS detection
3. Pingdom — availability and uptime monitoring
3. Jira — incident ticketing
4. Confluence — runbook and knowledge base documentation



⚠️ Note

All content is generic and does not include any confidential or company-specific information. Written for educational and portfolio purposes.


🤝 Connect


📧 swetharajendran4055@gmail.com

💼 LinkedIn — https://www.linkedin.com/in/swetha-rajendran-4349a12a1

🐙 GitHub — https://github.com/swetharajendran2015-a11y

🎥 YouTube — https://www.youtube.com/@alexessecurity
