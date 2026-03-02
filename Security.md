# Security Policy

## Reporting Vulnerabilities

If you discover a security issue in this repository or the examples it contains:

1. Submit a report via huntr: [https://huntr.com/bounties/disclose/](https://huntr.com/bounties/disclose/?target=https%3A%2F%2Fgithub.com%2FSubconscious-ai%2Fsublime&validSearch=true)
2. If huntr is unavailable or your report is time-sensitive, email: `security@subconscious.ai`

Please include:
- A clear description of the issue and impact
- Reproduction steps or proof of concept
- Any affected files, links, or endpoints

Do not open public GitHub issues for unpatched security vulnerabilities.

## Scope

This policy applies to content in this repository, including:
- Documentation and examples in `README.md`, `api/README.md`, and `antler_hackathon.md`
- Repository configuration and workflow files

Third-party services linked from this repository are out of scope unless the issue is caused by this repository's configuration or guidance.

## Secret Handling

- Never commit real credentials, API keys, client secrets, or tokens.
- Use placeholders such as `<username>`, `${SUBCONSCIOUS_TOKEN}`, and `<client_id>`.
- If credentials are accidentally committed, rotate them immediately and submit a private security report.

## Responsible Disclosure

We ask reporters to avoid public disclosure until the issue has been triaged and mitigated. We will acknowledge valid reports and coordinate remediation timelines.
