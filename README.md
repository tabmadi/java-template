# Java Template

A template for a Java 25 LTS application on Gradle with the Kotlin DSL. Every compiler warning is an error, Error Prone and NullAway run at compile time, and JaCoCo holds a coverage floor. It starts from [github-project-template](https://github.com/tabmadi/github-project-template), so it has the same hooks, CI gate, and documentation convention.

## Stack

| Tool | Owns |
| --- | --- |
| [Temurin](https://adoptium.net) JDK 25 | The runtime and the compile toolchain |
| [Gradle](https://gradle.org) wrapper | Build, run, and test |
| [Spotless](https://github.com/diffplug/spotless) | Format, with `palantir-java-format` |
| [Error Prone](https://errorprone.info) and [NullAway](https://github.com/uber/NullAway) | Static analysis and the null check |
| [JUnit](https://junit.org) and [AssertJ](https://assertj.github.io/doc/) | Tests |
| [JaCoCo](https://www.jacoco.org) | The coverage floor |
| [Typesafe Config](https://github.com/lightbend/config) | Configuration |
| [mise](https://mise.jdx.dev) | Tool versions and tasks |
| [lefthook](https://github.com/evilmartians/lefthook) | Git hooks |
| [cocogitto](https://docs.cocogitto.io) | Conventional Commits checks |
| [rumdl](https://github.com/rvben/rumdl) | Markdown lint |
| [gitleaks](https://github.com/gitleaks/gitleaks) | Secret scan |
| [Renovate](https://docs.renovatebot.com) | Updates to the pinned versions |

## Getting started

1. Install [mise](https://mise.jdx.dev/getting-started.html).
2. Clone the repo and install the JDK, the dependencies, and the hooks:

   ```bash
   git clone git@github.com:tabmadi/java-template.git
   cd java-template
   mise install
   mise run setup
   ```

3. Run the app:

   ```bash
   mise run run
   ```

## Tasks

| Task | Does |
| --- | --- |
| `mise run setup` | Compiles once to fill the dependency cache, then installs the git hooks |
| `mise run run` | Runs the app from source |
| `mise run build` | Builds the shaded jar at `bin/app.jar` |
| `mise run start` | Builds and runs the jar |
| `mise run format` | Applies every formatter |
| `mise run lint` | Runs `lint:java`, `lint:md`, and `lint:secrets` |
| `mise run test` | Runs the tests and the coverage floor |
| `mise run check` | Runs `lint` and `test`. Run it before you finish a change |
| `mise run ci` | Runs `check` and `build`, exactly as CI does. The `pre-push` hook runs it too |

## Layout

```text
.
├── src/main/java/io/github/tabmadi/app/
│   ├── Main.java             The entry point
│   └── config/AppConfig.java The configuration, parsed once into a record
├── src/main/resources/
│   ├── application.conf      The configuration defaults
│   └── logback.xml           The log format
├── src/test/java/            The JUnit tests
├── gradle/libs.versions.toml Every dependency and plugin version
├── build.gradle.kts          The build
├── docs/adr/                 The decisions: foundations, doc conventions, toolchain
├── .mise.toml                Tool pins and tasks
└── .lefthook.yml             Git hooks
```

## Configuration

`AppConfig` reads `application.conf`. An environment variable overrides each key.

| Variable | Description | Default |
| --- | --- | --- |
| `APP_NAME` | The name of the application | `java-template` |
| `LOG_LEVEL` | The log level | `INFO` |

## Using this template

1. Create a repository from this template on GitHub.
2. Change `group` and `version` in `build.gradle.kts` and `rootProject.name` in `settings.gradle.kts`.
3. Rename the `io.github.tabmadi.app` package. Change `mainClass` and the NullAway `annotatedPackages` in `build.gradle.kts` to match.
4. Update the contacts in `SECURITY.md` and the copyright line in `LICENSE`.
5. Install the [Renovate app](https://github.com/apps/renovate) on the new repository.

## Decisions

The ADRs in [docs/adr](docs/adr/README.md) record every decision and the reason for it. [ADR-0001](docs/adr/0001-documentation-conventions.md) is the house style for all text.

## Contributing

Follow the [Code of Conduct](CODE_OF_CONDUCT.md). Commit messages follow [Conventional Commits](https://www.conventionalcommits.org), and the `commit-msg` hook checks them. Run `mise run check` before you open a pull request.

## Security

To report a vulnerability, follow [SECURITY.md](SECURITY.md).

## License

Apache License 2.0. See [LICENSE](LICENSE).
