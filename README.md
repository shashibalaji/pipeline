# Selenium CI Pipelines

A Selenium + TestNG project whose real subject is **CI design in GitHub Actions**: the same Maven test suite is triggered in several different ways, with reusable pieces shared between workflows.

## Workflows

| Workflow | Trigger | What it shows |
|---|---|---|
| `test-pr-execution.yml` | Pull request | Gate PRs on the test suite |
| `test-and-deploy-nightly.yml` | Cron, weekdays | Scheduled regression |
| `pipeline1.yml` | Cron + manual | Daily run with manual override |
| `test-and-deploy-manually.yml` | `workflow_dispatch` | Choose browser (Chrome/Firefox) and target URL at run time |
| `test-and-deploy-cloud.yml` | `workflow_dispatch` | Run on a remote Selenium grid (Testinium) using secrets |
| `test-resusable-flow.yml` | `workflow_dispatch` | Same job built from **composite actions** in `.github/reusableFlows/` (setup + archive) |
| `sonar.yml` | Push / PR | SonarCloud static analysis (runs when `SONAR_TOKEN` and `SONAR_PROJECT_KEY` are configured) |

Surefire reports are uploaded as build artifacts on every run.

## The test

`AppTest` reads `browser` and `url` from system properties, so one test runs headless Chrome, headless Firefox or a remote grid browser without code changes.

## Run locally

```bash
mvn -B clean test -Dbrowser=Chrome -Durl=https://www.swtestacademy.com
```

## Tech

Java 17 · Selenium 4 · TestNG · Maven · GitHub Actions (cron, `workflow_dispatch` inputs, composite actions, secrets) · SonarCloud
