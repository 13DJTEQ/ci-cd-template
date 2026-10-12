# Security Policy

## Supported Versions

The repository owner maintains security fixes on the default branch
(`master`). No tagged releases are currently published, so only the
default branch is supported.

| Version  | Supported          |
|---------|--------------------|
| `master`| :white_check_mark: |
| `< 1.0` | :x: (no releases yet) |

## Reporting a Vulnerability

We take security vulnerabilities seriously. **Please do not report
security issues through the public issue tracker.**

To report a vulnerability, use one of the following:

- **GitHub Security Advisories (preferred):**
  Open a **private** security advisory at
  `https://github.com/13DJTEQ/ASUSRouterControl/security/advisories/new`
  (requires a GitHub account). This keeps the report off the public
  issue tracker.
- **Email:** `bavila@mediawave` (PGP: not available — request key via
  the advisory for sensitive details).

Please include in your report:

1. A clear description of the vulnerability and the potential impact.
2. Steps to reproduce (proof-of-concept if applicable).
3. Versions/commit SHAs affected.
4. Whether the vulnerability is already known to you (CVE / GHSA ID).

### Response timeline

- **Acknowledgment:** within 48 hours.
- **Initial assessment:** within 5 business days.
- **Fix / mitigation:** coordinated on a case-by-case basis.

We will keep you informed of progress and coordinate any public
disclosure. Please give us a reasonable embargo window (default 90 days)
before any public disclosure.

## Safe Handling

- Do not publicly disclose exploit details until a fix is released or the
  embargo period expires.
- Do not include secrets, tokens, or personal data in reproduction steps.
- When reporting a leaked secret, rotate it immediately after coordinating
  disclosure.

## Dependencies

This project uses [Dependabot](https://docs.github.com/code-security/dependabot)
for automated dependency scanning and the
[Dependency Review](.github/workflows/dependency-review.yml) action to
gate pull requests on vulnerable or unwanted licenses.

- **Code scanning:** [CodeQL](.github/workflows/codeql-analysis.yml) runs on
  pushes and pull requests to `master`.
- **Secret scanning:** enabled at the repository level; rotate any
  committed secret immediately and notify `bavila@mediawave`.

## Attribution

Reporters who disclose vulnerabilities responsibly may be acknowledged
in the relevant release notes (with their explicit consent).

## License note

This repository currently has no LICENSE file. By submitting a security
report, you agree that your submission is made available to the repository
maintainers for the sole purpose of fixing the issue, and you grant no
other rights or license.
