# Live Demo: AI-Assisted Attack Tree Generation from a DFD
## Using Claude with the HACME Cats Scenario

---

## SAMPLE DFD DESCRIPTION (paste this into Claude)

```
I need you to act as a threat modeling expert. Based on the following Data Flow Diagram (DFD) description for a retail e-commerce application called "HACME Cats", generate a detailed attack tree.

## System: HACME Cats — Organic Cat Food E-Commerce Platform

### Business Context
- Business: Selling organic food for cats through physical and online stores
- Industry: Retail
- Handles: PII, payment card data (PCI DSS scope), order history
- Internet-facing: Yes (public e-commerce site + mobile app)
- Internal users: ~50 employees (warehouse, customer support, marketing, IT)

### DFD Level 1 — Components and Data Flows

**External Entities:**
- [E1] Customer (web browser / mobile app)
- [E2] Payment Gateway (Stripe)
- [E3] Shipping Provider API (FedEx/UPS)
- [E4] Marketing Analytics (Google Analytics)
- [E5] Admin/Employee (internal portal)

**Processes:**
- [P1] Web Application Server (Node.js on AWS EC2)
- [P2] API Gateway (REST API — serves mobile app + SPA frontend)
- [P3] Authentication Service (OAuth 2.0 + JWT)
- [P4] Order Processing Engine
- [P5] Inventory Management System
- [P6] Payment Processing Module

**Data Stores:**
- [D1] Customer Database (PostgreSQL on AWS RDS) — PII, credentials, order history
- [D2] Product Catalog (PostgreSQL) — product info, pricing, images
- [D3] Order Database (PostgreSQL) — orders, shipping status, payment refs
- [D4] Session Store (Redis) — JWT tokens, session data
- [D5] Log Aggregator (ELK Stack) — application logs, access logs

**Data Flows:**
- E1 → P2: HTTPS requests (browse products, place orders, manage account)
- P2 → P3: Authentication/authorization requests
- P3 → D1: Credential validation, token storage
- P3 → D4: Session/token management
- P2 → P4: Order submissions
- P4 → P6: Payment processing requests
- P6 → E2: Payment authorization (PCI tokenized)
- P4 → E3: Shipping label generation, tracking requests
- P4 → D3: Order persistence
- P1 → D2: Product catalog queries
- P5 → D2: Inventory updates
- E5 → P1: Admin operations (via VPN + internal portal)
- P1 → E4: Analytics events (page views, conversions)
- All Processes → D5: Logging

### Trust Boundaries:
- TB1: Internet ↔ DMZ (WAF + Load Balancer)
- TB2: DMZ ↔ Application Tier
- TB3: Application Tier ↔ Database Tier (private subnet)
- TB4: Internal Network ↔ Application Tier (VPN)

---

## Instructions:
Generate an attack tree with the following structure:

**Root Goal:** Compromise HACME Cats and exfiltrate customer PII/payment data

For each branch, include:
1. The attack path (multi-step where applicable)
2. The relevant MITRE ATT&CK technique IDs
3. Likelihood (High/Medium/Low)
4. Required attacker capability level

Organize the tree by these primary attack vectors:
- Branch 1: External Web Application Attacks
- Branch 2: Supply Chain / Third-Party Compromise
- Branch 3: Credential-Based Attacks
- Branch 4: Insider Threat
- Branch 5: API Exploitation

For each leaf node, suggest one D3FEND defensive technique.

Format the output as a structured text tree using indentation.
```

---

## WHAT TO EXPECT FROM CLAUDE

Claude will generate something like:

```
ROOT: Compromise HACME Cats — Exfiltrate Customer PII/Payment Data
│
├── Branch 1: External Web Application Attacks
│   ├── 1.1 SQL Injection on Customer DB [T1190]
│   │   ├── Target: P2 (API Gateway) → D1 (Customer DB)
│   │   ├── Path: Inject via search/order params → bypass WAF → extract PII
│   │   ├── Likelihood: Medium | Capability: Intermediate
│   │   └── D3FEND: Database Query String Length Restriction (D3-DQSLR)
│   │
│   ├── 1.2 Stored XSS → Session Hijacking [T1189 → T1539]
│   │   ├── Target: Product reviews → steal admin JWT from D4
│   │   ├── Likelihood: Medium | Capability: Intermediate
│   │   └── D3FEND: Browser Sandboxing (D3-BRS)
│   ...
│
├── Branch 2: Supply Chain / Third-Party Compromise
│   ├── 2.1 Compromised npm dependency [T1195.002]
│   │   ├── Target: P1 Node.js app via malicious package update
│   │   ├── Path: Typosquat/compromise upstream → backdoor in build
│   │   ├── Likelihood: Medium | Capability: Advanced
│   │   └── D3FEND: Software Bill of Materials (D3-SBOM)
│   ...
│
├── Branch 3: Credential-Based Attacks
│   ├── 3.1 Credential Stuffing [T1110.004]
│   │   ├── Target: P3 Auth Service via P2 API Gateway
│   │   ├── Path: Leaked credential lists → automated login attempts
│   │   ├── Likelihood: High | Capability: Low
│   │   └── D3FEND: Multi-factor Authentication (D3-MFA)
│   ...
```

---

## DEMO FLOW (5 minutes)

1. **Show the DFD (~30 sec)**
   - Display the HACME Cats DFD description (or a visual diagram if you have one)

2. **Paste into Claude (~30 sec)**
   - Open claude.ai, paste the prompt
   - *"Watch how fast this generates vs. a manual brainstorming session"*

3. **Review the output (~2 min)**
   - Walk through 2-3 branches
   - Highlight the ATT&CK technique mapping
   - Show the D3FEND defensive recommendations
   - Point out: *"This took 30 seconds vs. hours of manual work"*

4. **Critical discussion (~2 min)**
   - *"Is this output perfect? No — it's a starting point"*
   - *"What did it miss? What would YOU add?"*
   - *"AI accelerates brainstorming but doesn't replace expert judgment"*
   - *"The AI doesn't know YOUR environment, YOUR controls, YOUR crown jewels"*
   - *"Always validate against real threat intelligence (your CTI from Day 1)"*

---

## TIPS FOR THE LIVE DEMO

- **Have the prompt pre-loaded** in a text file (don't type it live)
- **If Wi-Fi is unreliable**, have a pre-generated screenshot as backup
- **Ask audience:** *"What branch would you add that Claude missed?"*
- **Great segue into:** *"Now let's do this manually with ATT&CK Navigator..."*

---

## OPTIONAL FOLLOW-UP PROMPTS

After the initial attack tree, you can show these follow-up prompts:

### Follow-up 1 — Map to ATT&CK Navigator:

```
Now take the ATT&CK techniques from the attack tree above and generate
an ATT&CK Navigator JSON layer file that I can import. Color-code by
likelihood: Red=High, Yellow=Medium, Green=Low. Include comments for
each technique.
```

### Follow-up 2 — Generate detection rules:

```
For the top 5 highest-likelihood attack paths, generate Sigma detection
rules that would help detect each attack in an ELK Stack environment.
```

### Follow-up 3 — Risk scoring:

```
Apply DREAD scoring (Damage, Reproducibility, Exploitability, Affected Users,
Discoverability) to each branch of the attack tree. Score each factor 1-10
and calculate the average risk score. Present as a table sorted by risk.
```
