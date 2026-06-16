NOC Runbooks — DDoS Mitigation 🛡️

A collection of operational runbooks for common DDoS attack types handled in a live NOC environment.

Each runbook follows the same structure used in real NOC operations — from alert detection through to resolution and documentation. Written based on hands-on L3/L4 DDoS mitigation experience.


"An alert is not just a notification — it's a starting point for investigation."




📁 Runbooks

FileAttack TypeMitigation Approachudp-flood-null-route.mdUDP FloodNull Routesyn-flood-mitigation.mdSYN FloodSYN Cookie / Rate Limitingicmp-flood-response.mdICMP FloodICMP Rate Limiting / Blockack-flood-detection.mdACK FloodStateful Inspectionhttp-flood-cwaf.mdHTTP FloodCloud WAF Rule Enforcementddos-escalation-checklist.mdAll TypesEscalation Decision Tree


🔍 Runbook Structure

Every runbook contains:


What it is — attack definition and how it works
How it looks — traffic patterns, alert signatures, tool indicators
Verification steps — what to check in Grafana, Kentik, Pingdom
Mitigation actions — step-by-step response procedure
Escalation criteria — when to escalate and to whom
Documentation — how to log the incident in Jira and Confluence



🛠️ Tools Referenced


Grafana — traffic visualization and alert monitoring
Kentik — flow analysis and DDoS detection
Pingdom — availability and uptime monitoring
Jira — incident ticketing
Confluence — runbook and knowledge base documentation



⚠️ Note

All content is generic and does not include any confidential or company-specific information. Written for educational and portfolio purposes.


🤝 Connect


📧 swetharajendran4055@gmail.com
💼 LinkedIn https://www.linkedin.com/in/swetha-rajendran-4349a12a1/
🐙 GitHub Profile https://github.com/swetharajendran2015-a11y
