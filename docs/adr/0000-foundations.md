# ADR-0000: Foundations

**Status:** Accepted
**Decides:** What this repo is, its layout, its Java conventions, and the ADR process itself.

## Context

A Java project starts with the same choices every time: the build tool, the formatter, the static analysis, the null policy, and the test stack. Each has several popular answers. A template makes these choices once, so a new project starts at its first feature.

## Decision

This repo is a Java application template on Gradle with the Kotlin DSL, for Java 25 LTS. It starts from [github-project-template](https://github.com/tabmadi/github-project-template) and adds the Java toolchain. The sample code shows the structure. It is not a feature set.

| Path | Holds |
| --- | --- |
| `src/main/java/` | The application code, under `io.github.tabmadi.app` |
| `src/main/resources/application.conf` | The configuration defaults, in HOCON |
| `src/test/java/` | The JUnit tests |
| `gradle/libs.versions.toml` | Every dependency and plugin version |
| `gradlew` and `gradle/wrapper/` | The committed Gradle wrapper |

**Every version lives in the catalog.** `build.gradle.kts` references `libs.<alias>` and holds no version literal.

**Configuration is read in one place.** `AppConfig` reads `application.conf`, applies the environment variable overrides, and parses the result once into a record. No other class calls `System.getenv`.

**Warnings are errors.** The compiler runs with `-Xlint:all -Werror`, Error Prone, and NullAway, per [ADR-0100](0100-toolchain.md).

**The code is non-null by default.** NullAway treats `io.github.tabmadi` as annotated. A nullable reference carries the JSpecify `@Nullable`.

**Every decision is an ADR.** An ADR states what is true of the repo. It is law, not a plan. Planned work lives in an untracked `*.local.md` file.

## Consequences

A dependency added here is a dependency of every generated project. So the template adds one only when the template itself needs it.

The Java version is in two places: `.mise.toml` for the JDK that runs Gradle, and the catalog for the toolchain that compiles. A change updates both.

A generated project inherits these ADRs. So an ADR is written for the engineer who maintains the project. Selection guidance belongs in the root `README.md`.

## Rules

- Every dependency and plugin version is in `gradle/libs.versions.toml`.
- The build runs through `./gradlew`. The wrapper changes only through `gradle wrapper --gradle-version <v>`.
- Only `AppConfig` reads environment variables.
- A compiler warning fails the build. `(CI: lint:java)`
- A `@SuppressWarnings` carries a comment that says why the analysis is wrong at that line.
- A nullable reference carries `@Nullable`. `(CI: lint:java)`
- Tests use JUnit with AssertJ, and a `@DisplayName` that names the behaviour.
- Line coverage stays at or above the JaCoCo floor in `build.gradle.kts`. The floor is raised, never the exclusion list widened. `(CI: test)`
- Data types are records.
- Every ADR carries a `Status`, a one-sentence `Decides` line, and a flat `Rules` section.
- Planned work lives in an untracked `*.local.md` file. No committed file links to one.
