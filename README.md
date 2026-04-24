# Security Policy

Gradual Inc. ("Gradual", "we", "us") operates a multi-tenant community platform and takes the security of that platform seriously. This policy describes how security researchers can report vulnerabilities to us, what you can expect in return, and the terms under which good-faith security research is authorized.

By submitting a vulnerability report, you agree to the terms of this policy.

**Last updated:** 2026-04-24
**Policy version:** 1.0

---

## Reporting a Vulnerability

### Preferred channel

Submit a private security advisory via GitHub:

👉 **[github.com/Gradual-Inc/security/security/advisories/new](https://github.com/Gradual-Inc/security/security/advisories/new)**

Your submission is visible only to Gradual's security team. GitHub automatically adds you as a collaborator on the advisory, allowing private discussion with our team throughout triage and remediation.

### Alternative channel

Email: **security@gradual.com**

For sensitive reports, please encrypt with our PGP key:
- Key file: [pgp-key.txt](./pgp-key.txt) (in this repository)
- Fingerprint: `FD16 C9EC 0EE0 63F3 7D14  CC7B 8B37 4AB2 E030 BDF1`

### What to include

To help us triage quickly, please include:

- A clear description of the vulnerability
- The affected URL, endpoint, or component
- Step-by-step reproduction instructions
- A proof of concept (PoC), if applicable
- The potential impact
- Your suggested CVSS 3.1 score (optional but helpful)
- Your preferred name/handle for public credit (if desired)
- Whether you consent to public disclosure after remediation

Please do **not** include real customer data, production credentials, or the personal information of any Gradual employee, customer, or end-user.

---

## Scope

Gradual operates a multi-tenant community platform. Some customers point their own domains at our infrastructure. This makes scope slightly more nuanced than a typical VDP — please read this section carefully.

### In scope

Vulnerabilities in **Gradual-operated platform surfaces**, specifically:

- **`*.gradual.us`** and all subdomains, including tenant subdomains (e.g., `customer.gradual.us`)
- **Custom customer domains** that point at the Gradual platform (see [Conditionally in scope](#conditionally-in-scope--custom-customer-domains) below)
- The Gradual web application served on the surfaces above, including features such as communities, forums, membership, chat, video, and the content editor
- Gradual mobile applications published by Gradual
- Authentication and authorization flows (SSO, SAML, OIDC, session handling)
- Payment flows, where the vulnerability is in Gradual's integration (not in the payment processor itself)
- Email handling and deliverability features operated by Gradual
- Any other Gradual-operated service reachable via the surfaces above that processes customer data

Domains not listed here (including marketing sites, third-party vendor tooling, and any `gradual.com` properties) are **out of scope** unless we explicitly add them.

### Conditionally in scope — custom customer domains

Some Gradual customers host their community on their own domain (e.g., `community.customer-example.com` → CNAME → `customer.gradual.us`).

Vulnerabilities reachable via a customer's custom domain are **in scope only if the root cause is in the Gradual platform** (e.g., XSS, IDOR, authentication bypass, server-side issues). In this case:

- You **must not** test against a real customer's production domain or instance
- You **must** reproduce the issue on a Gradual-controlled surface (e.g., a `*.gradual.us` tenant you legitimately own, or a theoretical PoC agreed in advance with us)
- Report the issue as a Gradual platform vulnerability, not as a vulnerability affecting a specific customer

### Out of scope

The following are **not** considered vulnerabilities in the Gradual platform and are out of scope:

**Customer-controlled configuration:**
- DNS misconfigurations on customer-owned domains (dangling CNAMEs, missing CAA records, DMARC/SPF configuration)
- Subdomain takeovers where the root cause is a customer's stale DNS record pointing to a reclaimable Gradual subdomain
- TLS/SSL certificate issues on customer-managed custom domains
- Customer misconfiguration of their Gradual tenant settings

**Commonly excluded:**
- Social engineering attacks against Gradual employees, customers, or end-users
- Physical attacks against Gradual facilities or personnel
- Denial-of-service (DoS / DDoS) attacks, volumetric attacks, or resource exhaustion testing
- Rate-limiting bypasses without demonstrated security impact
- Automated scanner output without a demonstrated, exploitable PoC
- Brute-force or credential-stuffing attacks
- Content spoofing / text injection without demonstrated impact
- Self-XSS (requiring victim to paste malicious content into their own browser)
- Missing security headers without a working exploit
- Missing best-practice configurations (HSTS preload, CSP strictness, cookie flags) without demonstrated impact
- Clickjacking on pages with no sensitive state-changing actions
- CSRF on logout, low-impact endpoints, or endpoints with no side effects
- Disclosure of public information or non-sensitive internal paths
- Reports from outdated browsers or unsupported platforms
- Vulnerabilities affecting users of end-of-life browsers
- Email deliverability issues (SPF, DKIM, DMARC on third-party domains we don't control)
- Issues requiring a rooted / jailbroken / physically compromised device

**Third-party services:**
- Vulnerabilities in third-party services (Stripe, Postmark, Mux, Agora, Cloudflare, MongoDB Atlas, etc.) — please report these directly to the vendor
- Vulnerabilities in open-source dependencies (report to the upstream project; we'll coordinate on Gradual's side)

**Research conduct that is never authorized:**
- Accessing, modifying, or destroying data that does not belong to you
- Pivoting from your own tenant into any other customer's tenant
- Exfiltrating more data than necessary to demonstrate the vulnerability
- Using identified vulnerabilities for any purpose other than reporting them to us
- Publicly disclosing a vulnerability before we have had a reasonable opportunity to remediate (see [Coordinated Disclosure](#coordinated-disclosure))

---

## Testing Guidelines

**We do not provide dedicated test tenants.** To keep research safe for you and for our customers, please follow these rules:

1. **Test only against surfaces you already have legitimate access to.** If you are a member of a Gradual community, you may probe features that affect only your own account and data. Do not create fake accounts en masse or abuse signup flows on real customer communities.
2. **Never test against real customer tenants or custom domains.** If you believe a class of vulnerability affects the platform, describe it and, where you can, demonstrate it from public-facing surfaces or static analysis — do not actively exploit it against a production tenant.
3. **Coordinate before active testing.** If you need to perform active testing that goes beyond your own account, email `security@gradual.com` first and describe what you intend to do. We will tell you whether and how we can support it.
4. **Stop at proof.** Once you've demonstrated a vulnerability exists, stop. Do not continue exploiting it to access additional data.
5. **Don't harm customer data.** If you accidentally access data that doesn't belong to you, stop immediately, do not save or share it, and tell us in your report.
6. **Don't disrupt service.** No DoS, no resource exhaustion, no destructive testing.
7. **Stay within scope.** If you're unsure whether something is in scope, ask us at `security@gradual.com` before testing.

---

## Safe Harbor

Gradual supports security research conducted in good faith in accordance with this policy. For such research, we commit to the following:

- We will **not pursue or support legal action** against you for good-faith research that complies with this policy, including under applicable computer-misuse or anti-hacking laws (such as the U.S. Computer Fraud and Abuse Act, the U.K. Computer Misuse Act, or China's Cybersecurity Law).
- We will **not pursue copyright or DMCA claims** against you for circumvention of technical measures, to the extent your research complies with this policy.
- We will **work with you in good faith** to understand and resolve the issue promptly, and will not recommend or pursue law enforcement action against you for your research.
- If your research is consistent with this policy but a third party (such as a customer or a law enforcement agency) initiates legal action against you, we will make it known that your actions were authorized.

**Good-faith research** means research conducted in a manner that:

- Avoids harm to Gradual, our customers, or our customers' end-users
- Avoids privacy violations, destruction of data, and interruption of service
- Uses only the minimum level of access needed to demonstrate the vulnerability
- Promptly reports the vulnerability to us and does not disclose it publicly before we've had a reasonable opportunity to remediate
- Complies with all applicable laws

This safe harbor does **not** authorize research that falls outside this policy. If in doubt, contact `security@gradual.com` before proceeding.

This safe harbor is based on the [disclose.io](https://disclose.io) core terms.

---

## Our Commitments

When you report a vulnerability to us, we commit to the following service levels:

| Stage | Commitment |
|---|---|
| **Acknowledgment** | Within 3 business days of submission |
| **Triage decision** | Within 10 business days of submission |
| **Status updates** | At least every 14 days during remediation |
| **Fix timeline (Critical, CVSS 9.0+)** | Within 7 days |
| **Fix timeline (High, CVSS 7.0–8.9)** | Within 30 days |
| **Fix timeline (Medium, CVSS 4.0–6.9)** | Within 90 days |
| **Fix timeline (Low, CVSS <4.0)** | Within 180 days |
| **Public disclosure** | 90 days after initial report, or sooner by mutual agreement |

We'll do our best to meet or beat these timelines. If a fix requires longer (for example, due to architectural complexity), we'll tell you why and keep you informed.

### What we will do

- Respond in a timely, professional manner
- Validate and triage your report honestly, including accepting duplicates or out-of-scope findings with clear explanations
- Keep you informed of remediation progress
- Give you public credit for your report (unless you request otherwise)
- Coordinate a CVE assignment where appropriate
- Invite you to verify the fix before public disclosure

### What we ask of you

- Follow this policy
- Give us a reasonable opportunity to fix the issue before public disclosure
- Avoid privacy violations and service disruption
- Don't demand payment in exchange for disclosure

---

## Severity Classification

We use **CVSS 3.1** for severity scoring. Our scoring considers:

- Attack vector (network, adjacent, local, physical)
- Attack complexity
- Privileges required
- User interaction
- Scope change
- Confidentiality, integrity, and availability impact
- Environmental factors specific to a multi-tenant community platform (including cross-tenant impact)

If you provide a suggested CVSS score, we will consider it. We publish our own score with reasoning; if we disagree, we'll explain why.

---

## Recognition

Gradual operates a **Vulnerability Disclosure Program (VDP)**. We do not currently offer monetary rewards for vulnerability reports.

We recognize researchers through:

| Severity | Recognition |
|---|---|
| **Critical** | Hall of Fame entry + signed acknowledgment letter + Gradual swag + CVE (if applicable) + LinkedIn recommendation offer |
| **High** | Hall of Fame entry + signed acknowledgment letter + Gradual swag + CVE (if applicable) |
| **Medium** | Hall of Fame entry + Gradual stickers |
| **Low** | Hall of Fame entry |

**Hall of Fame:** [HALL_OF_FAME.md](./HALL_OF_FAME.md) (in this repository)

You'll be credited under the name or handle you specify in your report. You may decline public credit if you prefer.

**If we launch a paid bounty program in the future**, researchers who have contributed to our Hall of Fame will receive priority access. We will announce any such program publicly; we cannot retroactively pay for past reports.

---

## Coordinated Disclosure

We believe in coordinated disclosure. Our standard process:

1. **Private report** → you submit via GitHub advisory or email
2. **Acknowledgment** → we confirm receipt within 3 business days
3. **Triage** → we validate and score the issue within 10 business days
4. **Remediation** → we develop, test, and deploy a fix
5. **Verification** → you (optionally) verify the fix
6. **Disclosure** → we publish an advisory with your credit, typically 90 days after the initial report or 30 days after the fix is deployed to all customers, whichever is sooner
7. **CVE assignment** → we request a CVE through GitHub's CNA where applicable

If you'd like to publish your own writeup, we're happy to coordinate a release date with you. Please don't publish before we've had a reasonable opportunity to remediate (normally 90 days from initial report).

If we disagree on disclosure timing, we'll work with you in good faith to reach an acceptable outcome. Unilateral early disclosure will result in your report being declined for credit.

---

## Legal & Regulatory Notes

Gradual operates globally and is subject to laws in multiple jurisdictions, including the United States, European Union (GDPR), United Kingdom, and China.

- **Data handling:** Please do not access, download, or retain personal data belonging to Gradual customers or their end-users. If you inadvertently access such data, delete it immediately and report the circumstances in your submission.
- **Sanctions compliance:** Gradual cannot provide recognition (including swag) to individuals in jurisdictions subject to comprehensive U.S. sanctions. Public Hall of Fame credit remains available.
- **China Cybersecurity Law:** Gradual maintains infrastructure in China for certain customers. Vulnerabilities affecting our China deployment are handled in accordance with applicable regulations, including reporting obligations under the *Regulations on the Management of Security Vulnerabilities of Network Products* (RMSV) where required.
- **No circumvention of customer agreements:** This policy applies to Gradual-operated infrastructure. It does not authorize you to violate the terms of service between Gradual's customers and their own end-users.

---

## Out of Scope: Customer Domains and DNS

Because some of our customers use their own domains with Gradual, we want to be explicit about a common confusion:

- **If the vulnerability is in Gradual's code/infrastructure** and merely surfaces on a customer domain, it's a Gradual vulnerability and **in scope**
- **If the vulnerability is in the customer's DNS configuration, their TLS certificate management, or their own systems**, it's **out of scope** for us — please contact the customer directly
- **If you've found a dangling CNAME or stale DNS record pointing to Gradual**, the customer (not Gradual) owns that DNS record. We may be able to facilitate contact with the customer with your permission, but this is not a Gradual vulnerability.

---

## Contact

| Purpose | Contact |
|---|---|
| Vulnerability report (preferred) | [GitHub private advisory](https://github.com/Gradual-Inc/security/security/advisories/new) |
| Vulnerability report (email) | security@gradual.com |
| General security questions | security@gradual.com |
| PGP key | [pgp-key.txt](./pgp-key.txt) |
| Hall of Fame | [HALL_OF_FAME.md](./HALL_OF_FAME.md) |
| Languages | English |

---

## Acknowledgments

This policy draws on community best practices, including:

- [disclose.io](https://disclose.io) core safe-harbor language
- [security.txt](https://securitytxt.org) (RFC 9116)
- GitHub's coordinated vulnerability disclosure guidance

We are grateful to the security research community for helping keep Gradual and our customers safe.

---

*Thank you for helping us keep Gradual's communities safe.*
