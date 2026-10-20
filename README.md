# Dependabot template

A small, stack-adaptable Dependabot configuration with weekly checks, a short
cooldown, and grouped updates.

## Use in an existing repository

1. Copy `dependabot.yml` to `.github/dependabot.yml` in the target repository.
2. Keep the GitHub Actions entry only if the repository has workflows.
3. Uncomment and duplicate the ecosystem example for each stack you use. Set
   `package-ecosystem` and the directory containing its manifests or lockfiles.
4. Commit the configuration to the repository's default branch.

The file lives at the root of this template repository so Dependabot only runs
after you install it in a target repository. If you use GitHub's **Use this
template** button, move the file to `.github/dependabot.yml` in the new repository.

## Pick your ecosystems

| Stack | `package-ecosystem` | Typical file |
| --- | --- | --- |
| JavaScript / TypeScript (npm, pnpm, Yarn) | `npm` | `package.json` |
| Python (pip, Poetry, pip-tools, Pipenv) | `pip` | `requirements.txt`, `pyproject.toml`, `Pipfile` |
| Python (uv) | `uv` | `pyproject.toml`, `uv.lock` |
| Go | `gomod` | `go.mod` |
| Rust | `cargo` | `Cargo.toml` |
| Ruby | `bundler` | `Gemfile` |
| Java / Kotlin (Maven) | `maven` | `pom.xml` |
| Java / Kotlin (Gradle) | `gradle` | `build.gradle`, `build.gradle.kts` |
| .NET | `nuget` | `*.csproj`, `packages.config` |
| PHP | `composer` | `composer.json` |
| Docker | `docker` | `Dockerfile` |
| Terraform | `terraform` | `*.tf` |
| GitHub Actions | `github-actions` | `.github/workflows/*.yml` |

Directories are relative to the repository root. For GitHub Actions, use `/`.
Only enable ecosystems with files present in your repository.

## Monorepos

Use `/` for a workspace managed by a single root lockfile. For independent
projects sharing the same settings, replace `directory` with `directories`:

```yaml
  - package-ecosystem: pip
    directories:
      - "/services/api"
      - "/services/worker"
    schedule:
      interval: weekly
```

Use separate entries when schedules or other settings differ. Do not configure
overlapping directories for the same ecosystem and target branch.

## Update policy

- Weekly checks keep pull-request traffic predictable.
- A three-day cooldown delays newly released versions; the ecosystem example
  waits seven days for major versions where semantic-version cooldowns apply.
  Cooldowns do not delay security updates.
- The ecosystem example groups minor and patch version updates, leaving major
  version updates in individual pull requests. Ecosystems without semantic
  versioning may need different grouping rules.
- Security updates are grouped separately. Enable **Dependabot alerts** and
  **Dependabot security updates** in the target repository's security settings;
  the configuration alone does not enable them.
- There are no dependency ignores, private registry credentials, reviewers,
  labels, or project-specific paths. Add these only when a target repository
  needs them. Keep private registry secrets in Dependabot secrets.

Check ecosystem-specific support before copying optional settings:
[Dependabot options reference](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference).
