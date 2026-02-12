# Repository Guidelines

## Project Structure & Module Organization
This repository is documentation-first.

- `README.md`: Primary project overview, links, and community entry points.
- `api/README.md`: API quickstart and request examples.
- `Security.md`: Vulnerability reporting and security policy.
- `antler_hackathon.md`: Event-specific reference material.
- Root metadata: `LICENSE`, `CODE_OF_CONDUCT.md`, `CNAME`.

Keep new content close to its audience: API usage updates belong in `api/README.md`; organization or product messaging belongs in `README.md`.

## Build, Test, and Development Commands
There is no in-repo build pipeline or automated test suite currently.

- `rg -n "term" README.md api/README.md`: Find and update repeated wording quickly.
- `git status -sb`: Check branch state before and after edits.
- `git diff -- README.md api/README.md`: Review exactly what changed.
- `gh pr create --draft --fill`: Open a draft PR after pushing your branch.

For docs changes, treat link validity, snippet accuracy, and formatting consistency as the release gate.

## Coding Style & Naming Conventions
- Use clear Markdown headings (`##`, `###`) and short sections.
- Prefer concise, imperative prose and concrete examples.
- Use fenced code blocks for request snippets.
- Use lowercase, hyphenated branch names such as `42-api-doc-refresh`.
- Keep filenames descriptive and stable; avoid creating duplicate topic files.

## Testing Guidelines
No formal coverage target is defined for this repository.

- Manually verify API snippets before merging (request path, payload keys, auth header format).
- Confirm external links resolve and point to canonical docs.
- If you change example inputs/outputs, ensure narrative text matches code blocks.

## Commit & Pull Request Guidelines
Recent history uses short, imperative subjects (for example, `Update README.md`, `Create CNAME`, `chore: ...`). Follow that pattern.

- Commit format: `<type(optional)>: <brief imperative summary>`.
- Keep commits scoped to one topic (README, API docs, or policy update).
- PRs should include: purpose, changed files, linked issue (for example `Refs #42`), and any screenshots only when visual output changed.

## Security & Configuration Tips
Never commit real credentials, tokens, or private endpoints. In examples, prefer placeholders (`<username>`, `${ACCESS_TOKEN}`) and reference secure reporting flow in `Security.md`.
