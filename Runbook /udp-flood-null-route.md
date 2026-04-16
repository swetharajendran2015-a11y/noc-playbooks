# Runbook: UDP Flood Detection & BGP Null-Route Response

**Type:** DDoS Mitigation Runbook  
**Layer:** L3 / L4  
**Tools Referenced:** NetFlow, BGP  
**Severity:** P1 — Service Impacting

---

## What This Looks Like

You'll usually catch this one of two ways:
- An alert fires from your DDoS platform (Radware, Arbor, etc.) showing inbound traffic spiking on a single destination IP
- A customer calls in saying their service is completely unreachable

In NetFlow, a UDP flood looks like this:
- One destination IP receiving traffic from thousands of different source IPs (spoofed)
- Port is often random (high ephemeral ports) or targeting specific services (DNS/53, NTP/123)
- Packets are small and uniform — usually 64–512 bytes, just enough to saturate bandwidth
- PPS (packets per second) is the real killer here, not always raw Mbps

On your DDoS platform you'll see:
- Baseline deviation alerts (traffic 10x–100x above normal)
- Source IPs geographically scattered — a sign of a botnet or reflection attack
- Protocol breakdown showing near 100% UDP

---

## Possible Causes

| Cause | What it means |
|---|---|
| Volumetric UDP flood | Attacker sending raw UDP garbage to overwhelm bandwidth |
| UDP reflection (amplification) | Attacker spoofing victim's IP to open resolvers (DNS, NTP, SSDP) — victim gets the response traffic |
| Misconfigured upstream peer | Rare — usually rules this out early |

> In ISP environments, reflection attacks are more common than pure floods. Always check if the traffic is inbound on port 53/123 — that's a reflection indicator.

---

## How to Verify

**Step 1 — Check NetFlow for traffic breakdown**

Pull a flow summary for the destination IP. You're looking for:
src_ip: varied (100s or 1000s of unique IPs)
dst_ip: <victim IP>
protocol: UDP
dst_port: 53 or random high ports
bytes/pkt: small and consistent

**Step 2 — Confirm it's not legitimate traffic**

- Is this IP normally a high-traffic host? Check historical baseline.
- Is the PPS ratio abnormal (lots of packets, low data per packet)?
- Are the source IPs from ASNs that don't make sense for this customer's traffic profile?

**Step 3 — Check your DDoS platform**

On your DDoS dashboard:
- Navigate to the real-time attack view
- Look for the active protection policy on the victim IP
- Confirm the attack vector shows UDP flood or DNS/NTP amplification

**Step 4 — Confirm link utilization**

Check your upstream interface graphs. If you're seeing the link approaching saturation, you cannot clean this locally — you need upstream action (null-route or upstream scrubbing).

---

## Steps to Resolve

### Option A — Local rate limiting (small attack, link not saturated)

If your DDoS platform has an active policy, it may already be absorbing the attack. Verify:
- Protection policy is in **active blocking** mode (not just detection)
- Blocking rule matches the attack signature (UDP rate limit threshold)

### Option B — BGP Null-Route (link is saturated or platform is overwhelmed)

> Use this when the attack is big enough that it's impacting other customers on the same link. You're trading "victim's service is down" for "victim's service stays down but everyone else is okay."

**1. Identify the victim's /32 (or specific prefix being attacked)**
Victim IP: 203.0.113.45

**2. Announce a null-route via BGP to your upstream**

On your router (example — Cisco IOS): ip route 203.0.113.45 255.255.255.255 Null0
router bgp <your-ASN>
network 203.0.113.45 mask 255.255.255.255

Or if your upstream supports RTBH (Remote Triggered Black Hole): ip route 203.0.113.45 255.255.255.255 Null0 tag 666

(Tag 666 is the common RTBH community signal — confirm with your upstream)

**3. Notify the customer**

- Inform them that their IP is being null-routed
- Their service will be unreachable until the attack subsides
- Set expectations: typical UDP floods last 15 minutes to a few hours

**4. Monitor and remove when attack stops**

Watch your NetFlow and DDoS platform. Once attack traffic drops to baseline:
no ip route 203.0.113.45 255.255.255.255 Null0

Confirm traffic returns to normal before closing the ticket.

---

## Escalation Triggers

Escalate to senior NOC or security team if:
- Attack volume exceeds your upstream link capacity
- The null-route is not accepted by upstream peer
- Multiple customers are being impacted simultaneously
- Attack traffic is coming from your own network (compromised customer equipment)

---

## Key Learnings

- A UDP flood from thousands of sources = botnet or reflection. A UDP flood from a handful of sources = direct attack. The response is similar but the source tells you about attacker sophistication.
- Null-routing kills the victim's service. Always notify the customer before you do it — don't let them find out from their own monitoring.
- RTBH is your cleanest option when available. It pushes the drop upstream before traffic even hits your link.
- Small floods that don't saturate your link are often better handled by your scrubbing platform. Save null-routing for when you're protecting the rest of the network.
- Always document the attack start time, peak volume, and resolution time. This data is useful if the customer escalates or requests a report.

---

*Last updated: 2025 | Author: NOC Engineer (L2)*
