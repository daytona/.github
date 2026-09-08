# Security Policy

## Reporting a Vulnerability

At Daytona, we take security seriously. If you believe you have found a security vulnerability in any Daytona-owned repository or service, please report it responsibly.

**Please do NOT report security vulnerabilities through public GitHub issues.**

Report vulnerabilities through either channel:

- **GitHub**: use the "Report a vulnerability" button in the Security tab of the repository where you found the issue
- **Email**: [security@daytona.io](mailto:security@daytona.io), for issues affecting the Daytona platform or anything not tied to a specific repository

Please include:

- Description of the vulnerability
- Steps to reproduce
- Impact assessment
- Any relevant screenshots or proof-of-concept

We will acknowledge receipt within 5 business days and provide an initial assessment within 10 business days.

## Scope

**Tier 1: Daytona platform and official client software.** Assessed with priority and eligible for discretionary reward:

- app.daytona.io, including the web application and management interfaces
- All documented and undocumented API endpoints of the hosted service
- Sandbox runtime isolation: escape from sandbox to host, cross-tenant access, isolation boundary bypasses
- Authentication and authorization: SSO, API key management, session handling, privilege escalation across accounts or organizations
- Secrets management: scoped secret injection, unauthorized access to secrets, leakage across sandbox boundaries
- Official SDKs and CLI: the [daytona/clients](https://github.com/daytona/clients) repository

**Tier 2: maintained open-source repositories.** Actively maintained repositories under the daytona and daytonaio GitHub organizations, such as integrations. Valid reports are remediated and credited via a GitHub security advisory. Monetary rewards are not offered for this tier unless the finding also demonstrates impact to the hosted platform.

**Tier 3: out of scope.**

- Forks of third-party projects: report issues in upstream code to the upstream project
- Archived and legacy repositories: no fixes will be issued
- Content, documentation, sample, and demo repositories, except where a finding demonstrates impact to the hosted platform

The presence of this security policy on a repository does not, by itself, place that repository in reward scope.

## Excluded Submission Types

The following categories are excluded from this program. Reports in these categories will be closed without further assessment unless they demonstrate impact beyond what is described.

1. **In-sandbox privilege escalation, root access, or capability use** — Daytona sandboxes provide full root access within user-namespace isolation by design. Findings that chain to host escape or cross-sandbox access remain in scope.
2. **Findings within the reporter's own sandbox** that do not demonstrate impact beyond that sandbox's isolation boundary.
3. **Denial of service** — DoS, DDoS, resource exhaustion, volumetric testing, or network flooding.
4. **Rate limiting observations** that do not demonstrate resource exhaustion, financial impact, or abuse potential.
5. **Social engineering** — phishing, vishing, pretexting, or any form of social engineering targeting Daytona employees or users.
6. **Physical security testing** of offices, data centers, or personnel.
7. **Marketing and documentation sites** — findings against daytona.io or docs.daytona.io, excluding subdomain takeover vulnerabilities.
8. **Third-party services** — vulnerabilities in services or platforms not owned or operated by Daytona.
9. **Known public files or directories** — e.g., robots.txt, .well-known, or other intentionally public resources.
10. **DNSSEC or TLS cipher suite configuration suggestions** without a demonstrated exploit path.
11. **Missing Secure/HTTPOnly flags** on non-sensitive cookies.
12. **CSRF on unauthenticated or public-facing forms.**
13. **Outdated browsers and platforms** — vulnerabilities only affecting unpatched or end-of-life software.
14. **Automated scan output** — reports generated solely by automated tools without validated proof of impact.
15. **Best practice recommendations** without demonstrable security impact.
16. **Spam or service degradation** — testing that results in sending unsolicited messages or degradation of service to other users.

## Supported Versions

The hosted Daytona platform is continuously deployed; reports are assessed against current production. For open-source repositories we accept reports against the latest released version.

## Safe Harbor

Daytona supports safe harbor for security researchers who act in good faith and in accordance with this policy.

We will not pursue legal action against researchers who:

- Make a good-faith effort to avoid privacy violations, data destruction, and service disruption
- Only access data to the extent necessary to demonstrate the vulnerability
- Test only against organizations, accounts, and sandboxes they own or created for testing; on encountering data belonging to another tenant, stop immediately, do not access it further, and include that in the report
- Do not exfiltrate, retain, or disclose any user data encountered during research
- Report findings promptly through the channels listed above
- Do not disclose findings publicly before coordinated resolution (see Disclosure Timeline below)
- Comply with all applicable laws

If legal action is initiated by a third party against a researcher for activities conducted in accordance with this policy, we will take steps to make it known that the research was authorized.

This safe harbor applies to all Daytona services and assets listed in the Scope section.

## Disclosure Timeline

We follow a coordinated disclosure process:

- **90 days** — We target remediation within 90 days of a validated report. Complex issues may require additional time, and we will communicate timelines transparently.
- **30 days post-patch** — After a fix is released, we ask that researchers wait 30 days before public disclosure to allow users to update.
- **No response** — If we fail to acknowledge or respond to a report within 90 days, the researcher may proceed with public disclosure after providing 14 days advance written notice to security@daytona.io.

## Rewards

We offer rewards from $100 to $1,000 for valid, original findings that demonstrate real security impact to the Daytona platform (Tier 1). Severity, exploitability, and report quality are all considered. Rewards are not offered for Tier 2 or Tier 3 reports. Duplicate reports are credited to the first submission.

## SSH Host Key Verification

Sandbox SSH connections terminate at `ssh.app.daytona.io`. The gateway presents a single Ed25519 host key, published here so that clients can verify it out of band rather than accepting it on first connection.

```
SHA256:A1Q6sAQohGVYSNJt8rInS/gjF3Hd0Yp33d0HymhdEV8
```

As a `known_hosts` entry:

```
ssh.app.daytona.io ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIM9Fx4ppJ0ehZyJkC8dYxsgN7aXgF+74ry9M5h2rHVJg
```

To check what the gateway is presenting against the fingerprint above:

```
ssh-keyscan ssh.app.daytona.io 2>/dev/null | ssh-keygen -lf -
```

A mismatch means the connection is not terminating at our gateway. Do not accept the key, and report it to [security@daytona.io](mailto:security@daytona.io).

Clients that support strict, non-interactive verification should seed the entry above rather than relying on trust on first use. Tooling that invokes OpenSSH inherits your system trust, so an entry in `~/.ssh/known_hosts` applies; tooling that keeps its own `known_hosts` file should be pointed at one containing the entry above.

**Rotation.** When this key is rotated, the replacement is published here before it is served, and both keys remain published for at least 30 days afterward, so a pinned client is never left with only a retired key. `known_hosts` accepts multiple entries for the same host, so both can be seeded at once.

**Scope.** This key covers `ssh.app.daytona.io`, the gateway used by Daytona's shared regions. Organizations on a dedicated or self-hosted region connect to a gateway with its own host key, published to those organizations directly.
