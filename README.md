# Open Food Facts `.github`

This is the organization-wide configuration and defaults repository for **Open Food Facts** on GitHub. Files placed here are automatically inherited by all repositories in the `openfoodfacts` organization unless overridden locally in a specific repository.

## What's Inside

| Path | Purpose | Scope |
| --- | --- | --- |
| [`profile/README.md`](profile/README.md) | Public organization profile shown at [github.com/openfoodfacts](https://github.com/openfoodfacts) | Organization profile |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Default community Code of Conduct | Org-wide default |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Default contributor & PR guidelines | Org-wide default |
| [`SUPPORT.md`](SUPPORT.md) | Default support channels (Slack, Discussions, Wiki) | Org-wide default |
| [`ACCESSIBILITY.md`](ACCESSIBILITY.md) | Default accessibility commitments and reporting | Org-wide default |
| [`SECURITY.md`](SECURITY.md) | Default security vulnerability reporting policy | Org-wide default |
| [`.github/VULNERABILITY_REPORT.yml`](.github/VULNERABILITY_REPORT.yml) | Structured form for GitHub Private Vulnerability Reporting | Org-wide default |
| [`.github/FUNDING.yml`](.github/FUNDING.yml) | Organization sponsor links | Org-wide default |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/) | Default Issue Forms (`bug_report.yml`, `feature_request.yml`, `config.yml`, etc.) | Org-wide default |
| [`.github/pull_request_template.md`](.github/pull_request_template.md) | Default Pull Request template | Org-wide default |
| [`.github/DISCUSSION_TEMPLATE/`](.github/DISCUSSION_TEMPLATE/) | Default GitHub Discussions category forms (`ideas.yml`, `q-and-a.yml`) | Org-wide default |
| [`workflow-templates/`](workflow-templates/) | Starter GitHub Actions workflows shown in **Actions → New workflow** | Org-wide picker |
| [`agents/`](agents/) | Organization-scoped GitHub Copilot custom agents | Org-wide Copilot |
| [`docs/`](docs/) | Cross-repo engineering conventions, port allocations, and Architecture Decision Records (ADRs) | Reference docs |

## Overriding Defaults in a Repository

Any repository in `openfoodfacts` can override a default community health file by adding its own file of the same type (for example, its own `CONTRIBUTING.md`, `.github/pull_request_template.md`, or `.github/ISSUE_TEMPLATE/`).

> **Note:** GitHub does not inherit `LICENSE`, `CODEOWNERS`, or `dependabot.yml` from `.github`—those must be added to each repository individually.
