# MT Troupe Logistics — Simulated IAM Environment

A fictional logistics company built to mirror MT Troupe Logistics, used as a hands-on
portfolio project for identity and access management (IAM) analyst / identity security
engineer roles. The goal is a single coherent narrative — one company, one identity
architecture — rather than a set of disconnected labs.

## Company Profile

Troupe Freight Solutions is a small logistics company with two distinct identity
populations that require different access models:

- **Workers (internal identities):** dispatch staff, ops/admin, and a rotating pool of
  independent contractor drivers. Contractors churn frequently and are the main
  joiner-mover-leaver (JML) challenge.
- **Clients (external identities):** brokers and customers who need scoped access to a
  load/shipment portal, but no access to internal systems.

## Architecture

| Layer | Tool | Purpose |
|---|---|---|
| Identity governance | MidPoint | System of record for worker identities, JML lifecycle rules |
| Identity provider (SSO) | Okta | Authentication for internal apps |
| External/client access | Okta (separate org or group) | Scoped, least-privilege portal access for brokers/customers |
| Privileged access | HashiCorp Vault | Short-lived credentials for sensitive systems (e.g. ops database) |
| Cloud IAM | AWS IAM + IAMGuard | Cloud-layer access, continuously audited |
| Documentation | Markdown playbooks + review logs | Access review process, offboarding, audit evidence |

## Build Phases

### Phase 1 — JML Lifecycle (Workers)
- [ ] Define three worker identity types in MidPoint: dispatch/ops (permanent),
      admin (permanent), contractor driver (temporary, contract-bound)
- [ ] Provisioning rule: hire event → MidPoint creates identity → auto-assigns Okta
      account + baseline app access by role
- [ ] Deprovisioning rule: contractor end-date reached → automatic access revocation
- [ ] Mover scenario: role change (e.g. dispatcher → ops lead) triggers access
      adjustment, not accumulation

### Phase 2 — Client/External Access Tier
- [ ] Separate identity population for brokers/customers (distinct Okta group or org)
- [ ] Scoped access: brokers see only their own loads, nothing internal
- [ ] Document the internal-vs-external separation decision and why it matters

### Phase 3 — Documentation Layer
- [ ] Quarterly access review process: who reviews, what's checked, how findings
      get remediated
- [ ] Playbook: contractor offboarding
- [ ] Playbook: new-hire provisioning
- [ ] Playbook: access review walkthrough
- [ ] Audit evidence log tying back to Phase 1/2 actions

## Related Projects (existing)

- **IAMGuard** — open-source AWS IAM misconfiguration scanner
  (github.com/marshallroberson24-ux/iamguard)
- **PAM + Zero Trust Access Lab** — HashiCorp Vault issuing short-lived PostgreSQL
  credentials (github.com/marshallroberson24-ux/pam-zero-trust-lab)

## Interview Narrative

"I built a simulated logistics company modeled on the one I actually run access for
at work. Workers are provisioned through MidPoint and Okta with automated
joiner-mover-leaver rules, contractor offboarding is automatic on contract end,
privileged database access is issued short-lived through Vault instead of standing
access, and I run quarterly access reviews documented against written playbooks."
