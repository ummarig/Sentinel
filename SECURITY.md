# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.x     | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

**Do not open public issues for security vulnerabilities.**

If you discover a security vulnerability in Sentinel, please report it privately:

- **Email**: security@sentinel-security-productions.com
- **GitHub Security Advisory**: [Report via GitHub](https://github.com/sentinel-security-productions/Sentinel/security/advisories/new)

Include the following details in your report:

- A clear description of the vulnerability
- Steps to reproduce the issue
- Potential impact and affected components
- Any suggested fixes or mitigations

## Response Timeline

| Stage              | Timeline        |
| ------------------ | --------------- |
| Acknowledgment     | 48 hours        |
| Initial Assessment | 5 business days |
| Status Update      | Every 7 days    |
| Fix Release        | 30-90 days      |

## Safe Harbor

We consider security research conducted in good faith to be authorized activity. We will not pursue legal action against researchers who:

- Make a good faith effort to avoid privacy violations and service disruptions
- Do not exploit the vulnerability beyond proof of concept
- Report findings directly and do not disclose publicly until a patch is available

## Disclosure Guidelines

### Responsible Disclosure

1. **Private Reporting**: All vulnerabilities must be reported privately through the channels listed above.
2. **Embargo Period**: Please allow us at least 90 days to release a patch before public disclosure.
3. **Patch Coordination**: We may coordinate with you on the timing and details of public disclosure.

### Scope

Vulnerabilities in the following areas are in scope:

- Code in the `apps/`, `libs/`, `database/`, and `prisma/` directories
- CI/CD and deployment configurations in `.github/`
- Authentication and authorization flows
- Data handling and storage mechanisms

Out of scope:

- Social engineering of team members
- Physical attacks on predecessor systems
- Denial of service on testnet infrastructure

## Security Updates

Security patches are released as patch versions (e.g., `1.0.1`, `1.0.2`). We recommend always running the latest patch version for the best security posture.
