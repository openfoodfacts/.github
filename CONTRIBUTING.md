# Contributing to Open Food Facts

Thank you for your interest in contributing to **Open Food Facts**! Every contribution—whether code, documentation, design, translations, or bug reports—helps make food data open and accessible to everyone.

## Getting Started

1. **Say hello on Slack:** Join [slack.openfoodfacts.org](https://slack.openfoodfacts.org/) and introduce yourself. It is the fastest way to find a project that matches your interests and get help setting up.
2. **Read the Tech Team guide:** Visit the [Open Food Facts Tech Team wiki](https://wiki.openfoodfacts.org/Tech_Team) for an overview of our architecture, repositories, and onboarding tips.
3. **Find an issue:** Look for issues labeled `good first issue` or `help wanted` in the repository you want to contribute to. Before starting work on an older issue, check that it is still reproducible and not already being worked on.

## How to Contribute

### Reporting Bugs & Requesting Features

- Search existing open and closed issues first to avoid duplicates.
- Use the repository's issue forms (Bug report, Feature request, etc.) and include clear reproduction steps, environment details, and screenshots where helpful.

### Submitting a Pull Request

1. **Fork & branch:** Fork the repository and create a dedicated feature or fix branch from the default branch.
2. **Follow project conventions:**
   - Check the repository's `README.md` and local `CONTRIBUTING.md` (if present) for stack-specific setup, linters, and test commands.
   - Many Open Food Facts projects share standard `Makefile` targets (`make dev`, `make test`, `make up`, `make down`) and `.env` / `.envrc` conventions—see [General Project Conventions](https://github.com/openfoodfacts/.github/blob/main/docs/general-project-conventions.md).
3. **Use Conventional Commits:** Format your PR title (and commit messages) using [Conventional Commits](https://www.conventionalcommits.org/), e.g.:
   - `feat: add barcode scanner fallback`
   - `fix: handle missing nutrition grade`
   - `docs: update local Docker setup`
   - `ci: pin GitHub Actions versions`
4. **Keep PRs focused:** One logical change per PR makes reviews faster and easier. Include tests and documentation updates when changing behavior, and add before/after screenshots for UI changes.
5. **Be patient and responsive:** Maintainers are often volunteers. Respond to review feedback constructively and mark conversations resolved once addressed.

## Translations

You do not need to write code to help translate Open Food Facts! Visit [translate.openfoodfacts.org](https://translate.openfoodfacts.org/) to contribute translations via Crowdin.

## Community Standards

All contributors are expected to follow our [Code of Conduct](https://github.com/openfoodfacts/.github/blob/main/CODE_OF_CONDUCT.md).
