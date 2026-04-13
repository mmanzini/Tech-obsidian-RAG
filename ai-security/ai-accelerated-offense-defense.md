# Preparing for AI-Accelerated Offense

A seven-point defensive playbook from Anthropic's security teams, based on findings from Project Glasswing — using frontier models to find vulnerabilities in real codebases. The core thesis: within 24 months, AI will surface vast numbers of previously-unnoticed bugs, so defenders must adopt AI tools at the same pace as attackers.

Source: Anthropic, [Preparing your security program for AI-accelerated offense](https://claude.com/blog/preparing-your-security-program-for-ai-accelerated-offense)

## Context: Project Glasswing

- Anthropic's initiative to use Claude Mythos Preview for defensive cybersecurity
- AI models are rapidly reducing the resources, time, and skill required to find and exploit vulnerabilities
- Sub-Mythos-level models already find serious vulnerabilities that traditional reviews missed for years
- Patch-to-exploit window is shrinking — friction-based defences (tedious pivot hops, rate limits, SMS MFA) no longer hold against AI adversaries with unlimited patience

## The Seven Priorities

### 1. Close Your Patch Gap
- Patch CISA KEV (Known Exploited Vulnerabilities) catalogue items immediately
- Use EPSS (Exploit Prediction Scoring System) to prioritise the rest
- Internet-facing apps: patch within 24 hours of exploit availability
- Automate deployment and reboots where outage risk is acceptable
- **AI angle:** models excel at reversing a patch into a working exploit — the window is shrinking to hours

### 2. Prepare for Higher Vulnerability Volume
- Plan for order-of-magnitude increase in finding volume over the next ~2 years
- Check open-source dependency security with OpenSSF Scorecard
- Apply the same expectations to vendors
- **AI-assisted:** triage automation, dependency redundancy analysis, upgrade automation, vendoring unmaintained dependencies via LLM reimplementation

### 3. Find Bugs Before Shipping
- Static analysis + AI code review in CI, blocking merges on high-confidence findings
- Automated penetration testing in CD pipeline
- Secure the build pipeline (SLSA framework)
- Adopt CISA Secure by Design practices
- Prefer memory-safe languages (Rust, Go, managed runtimes) for new code
- **AI-assisted:** scan your own code with the same models attackers would use; LLM-generated patches for SAST findings; LLM-assisted memory-safe migration

### 4. Find Vulnerabilities Already in Your Code
- Prioritise by exposure: code parsing untrusted input, auth decisions, internet-reachable paths
- Include legacy code — least recent scrutiny = most to gain
- Budget engineering time for remediation
- **Practical:** pick one internet-facing service, scan its input handling and auth logic, use findings to estimate broader programme cost

### 5. Design for Breach
- Zero trust architecture (CISA maturity model, NCSC principles)
- Tie access to verified hardware, not credentials alone (FIDO2/passkeys)
- Isolate services by cryptographic identity — each workload carries its own identity
- Replace long-lived secrets with short-lived, narrowly-scoped tokens
- **Key insight:** friction-based controls degrade against AI adversaries; favour controls that hold even with unlimited attacker patience (hardware-bound creds, expiring tokens, network paths that don't exist)

### 6. Reduce and Inventory Exposure
- Maintain current inventory of every internet-facing host, service, API endpoint
- Decommission unused/legacy systems
- Minimise exposed surface: default-deny ingress, limit API surface
- **AI-assisted:** LLM + codebase + traffic logs to identify unused endpoints; autonomous external red-teaming (AI offensive agent against your own perimeter)

### 7. Shorten Incident Response Time
- AI triage agent at front of alert queue (read-only SIEM access, 100% coverage)
- Instrument dwell time and coverage as priority metrics
- Automate incident bookkeeping (notes, artifacts, parallel investigation, postmortem drafting)
- Humans make containment/disclosure/comms calls; AI handles evidence collection and write-ups
- Run tabletop exercises for five simultaneous incidents
- Map detection coverage against MITRE ATT&CK; prioritise lateral movement and credential access
- Establish emergency change procedures in advance

## Vulnerability Reporting Best Practices

When submitting reports upstream (especially to open-source maintainers receiving high volumes of low-quality AI-generated reports):
- State bug and impact in plain language in first paragraph
- Walk through the code path: where input enters, where mishandled, where consequence occurs
- Provide working reproduction (PoC or failing test)
- Include a proposed patch
- **Disclose AI involvement upfront** — concealing it costs more credibility than disclosing
- Self-check: can you explain the bug from memory without referring to model output?

## For Organisations Without a Security Team

- Turn on automatic updates everywhere
- Prefer managed services over self-hosting
- Use passkeys or hardware security keys
- Enable free code host security tooling (Dependabot, secret scanning, CodeQL)
- Publish a `SECURITY.md` if you maintain open-source projects

## Key Takeaways

- The patch-to-exploit window is collapsing — speed of patching is now the primary risk factor
- Friction-based defences (rate limits, non-standard ports, SMS MFA) degrade against AI adversaries; invest in hard barriers (hardware identity, short-lived tokens, zero trust)
- Scan your own code with frontier models before attackers do — this is the single highest-impact action
- Vulnerability volume will increase by an order of magnitude; automate triage and remediation tracking
- AI is simultaneously the threat and the most powerful defence tool — adoption speed determines which side benefits more

## Reference Frameworks

| Domain | Standards |
|---|---|
| Patch prioritisation | CISA KEV, FIRST EPSS |
| Baseline controls | ACSC Essential Eight, CISA CPGs, CIS Controls v8 |
| Secure development | NIST SSDF, OWASP ASVS/SAMM, CISA Secure by Design |
| Supply chain | SLSA, OpenSSF Scorecards, CISA SBOM |
| Zero trust | CISA ZT Maturity Model, NIST SP 800-207, NCSC ZT Principles |
| Detection & response | MITRE ATT&CK, MITRE D3FEND |

## Related

- [[ci-integrations/_index|CI Integrations]] — AI-powered code review as a pre-ship security layer
- [[constitutional-ai/_index|Constitutional AI]] — hard constraints on Claude's security-related behaviour
