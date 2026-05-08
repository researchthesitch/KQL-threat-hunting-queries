# Entra ID Threat Hunting Pack

A small, growing collection of KQL hunting queries for detecting identity-layer threats in Microsoft Entra ID (Azure AD), runnable in **Microsoft Sentinel** and adaptable to **Microsoft Defender XDR Advanced Hunting**.

Each query is mapped to MITRE ATT&CK, includes a description of the attacker behavior it surfaces, common false-positive sources, and references to the underlying Microsoft documentation.

## Why this exists

Identity is the new perimeter. The vast majority of cloud breaches in recent years have started with credential abuse, illicit consent grants, or token theft against Entra ID — not with traditional endpoint or network compromise. This repo is a study notebook and starter kit for hunting those threats.

## Coverage

| # | Detection | MITRE ATT&CK | Data Source |
|---|---|---|---|
| 01 | Password spray from a single IP | T1110.003 | `SigninLogs` |
| 02 | Risky sign-ins flagged by Identity Protection | T1078.004 | `SigninLogs` |
| 03 | Illicit OAuth consent grant | T1528 | `AuditLogs` |
| 04 | Privileged role assignment | T1098.003 / T1078.004 | `AuditLogs` |
| 05 | Successful legacy authentication | T1078.004 | `SigninLogs` |
| 06 | MFA method tampering by another user | T1556.006 | `AuditLogs` |

## Requirements

- A Microsoft Entra ID tenant with sign-in and audit logs flowing to a Log Analytics workspace, or
- A Microsoft Defender XDR tenant with Advanced Hunting enabled.

You can build a free lab in minutes:

1. Sign up for the [Microsoft 365 Developer Program](https://developer.microsoft.com/microsoft-365/dev-program) — free E5 tenant with 25 user licenses.
2. In Entra ID, configure **Diagnostic settings** to send `SignInLogs` and `AuditLogs` to a new Log Analytics workspace (or enable a Sentinel free trial on top of it).
3. Paste any query from `/queries` into Sentinel **Logs** and run.

## How to use

- Start with default lookback windows; tune `ago(...)` and thresholds to your environment.
- Treat every result as a *lead*, not a verdict — read the false-positives section in each query header before escalating.
- For Defender XDR Advanced Hunting, the table names differ (`AADSignInEventsBeta`, `IdentityLogonEvents`, etc.) but the field semantics are nearly identical; porting is straightforward.

## Repository structure

```
entra-id-hunting-pack/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
└── queries/
    ├── 01-password-spray.kql
    ├── 02-risky-signins.kql
    ├── 03-illicit-oauth-consent.kql
    ├── 04-privileged-role-assignment.kql
    ├── 05-legacy-auth-success.kql
    └── 06-mfa-method-tampering.kql
```

## Roadmap

- Sigma rule equivalents for SIEM-agnostic deployment
- Defender XDR Advanced Hunting variants alongside each Sentinel query
- Conditional Access policy-as-code companion repo
- Automated tuning notebook (Jupyter + KQL magic) for baselining thresholds per tenant

## License

MIT — see [LICENSE](LICENSE).
