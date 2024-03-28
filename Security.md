# Security Policy

## Reporting OSS Vulnerabilities

Subconscious is partnered with [huntr by Protect AI](https://huntr.com/) to provide 
a bounty program for our open source projects. 

Please report security vulnerabilities associated with the Subconscious 
open source projects by visiting the following link:

[https://huntr.com/bounties/disclose/](https://huntr.com/bounties/disclose/?target=https%3A%2F%2Fgithub.com%2FSubconscious-ai%2FSubconscious&validSearch=true)

Before reporting a vulnerability, please review:

1) In-Scope Targets and Out-of-Scope Targets below.
2) The [Subconscious-ai/Subconscious](https://python.Subconscious.com/docs/contributing/repo_structure) monorepo structure.
3) Subconscious [security guidelines](https://python.Subconscious.com/docs/security) to
   understand what we consider to be a security vulnerability vs. developer
   responsibility.

### In-Scope Targets

The following packages and repositories are eligible for bug bounties:

- Subconscious-core
- Subconscious (see exceptions)
- Subconscious-community (see exceptions)
- langgraph
- langserve

### Out of Scope Targets

All out of scope targets defined by huntr as well as:

- **Subconscious-experimental**: This repository is for experimental code and is not
  eligible for bug bounties, bug reports to it will be marked as interesting or waste of
  time and published with no bounty attached.
- **tools**: Tools in either Subconscious or Subconscious-community are not eligible for bug
  bounties. This includes the following directories
  - Subconscious/tools
  - Subconscious-community/tools
  - Please review our [security guidelines](https://python.Subconscious.com/docs/security)
    for more details, but generally tools interact with the real world. Developers are
    expected to understand the security implications of their code and are responsible
    for the security of their tools.
- Code documented with security notices. This will be decided done on a case by
  case basis, but likely will not be eligible for a bounty as the code is already
  documented with guidelines for developers that should be followed for making their
  application secure.
- Any LangSmith related repositories or APIs see below.

## Reporting LangSmith Vulnerabilities

Please report security vulnerabilities associated with LangSmith by email to `security@Subconscious.dev`.

- LangSmith site: https://smith.Subconscious.com
- SDK client: https://github.com/Subconscious-ai/langsmith-sdk

### Other Security Concerns

For any other security concerns, please contact us at `security@Subconscious.dev`.
