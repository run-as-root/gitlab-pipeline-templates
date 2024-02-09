# Gitlab Pipeline Templates

This Repository is a collection of useful reusable GitLab-Ci Step-Templates. No need to add the same CI-Steps into your 
projects over and over again. Simply use the 
[GitLab Include Feature](https://docs.gitlab.com/ee/ci/yaml/README.html#include) and the steps that you think are 
necessary for your specific project.

## Default Stages

Unfortunately, GitLab requires adding stages manually to the `.gitlab-ci.yaml`. This means, that it is not possible to 
use a pipeline step without having the stage specified. We provide a set of pre-defined stages that you might want to 
use as well.

- 📦 - Preparation
- 🌈 - Linting
- 🏰 - Static Tests
- 🏛 - Architecture
- 🦄 - Automated Testing
- 📄 - Documentation
- 🧱 - Pre Deployment
- 🚀 - Deployment
- ☕️ - Post Deployment

### 📦 - Preparation

The preparation stage runs at the beginning and usually takes care of tasks that have to be done in all steps. We use 
this to cache composer, install a project, seed a database or similar things.

The following steps are included in this stage.

- [Composer Setup](/preperation/composer.yml) - setting up a composer cache and perform a composer install

### 🌈 - Linting

we try to follow the aspect of **Fail fast, fail loud**. Linters are usually the fastest CI steps as there is no logic 
included but simple syntax checks. Linting Steps can be `failable` and should only run when the specific file type was
changed in the last commit or when the previous built failed.

The following linters are provisioned already:

- [JSON](/linter/json.yaml) - Performs Syntax Checks of the file type `.json`.
- [Markdown Spellcheck](/linter/markdown_spellcheck.yaml) - Performs Syntax Checks of the file type `.md`.
- [XML](/linter/xml.yaml) - Performs Syntax Checks of the file type `.xml`.
- [YAML](/linter/yaml.yaml) - Performs Syntax Checks of the file type `.yaml`.
- [PHP 7.1](/linter/php7.1.yaml) - Performs Syntax Checks of the file type `.php`.
- [PHP 7.2](/linter/php7.2.yaml) - Performs Syntax Checks of the file type `.php`.
- [PHP 7.3](/linter/php7.3.yaml) - Performs Syntax Checks of the file type `.php`.
- [PHP 7.4](/linter/php7.4.yaml) - Performs Syntax Checks of the file type `.php`.
- [PHP 8.0](/linter/php8.0.yaml) - Performs Syntax Checks of the file type `.php`.
- [PHP 8.1](/linter/php8.1.yaml) - Performs Syntax Checks of the file type `.php`.
- [PHP 8.2](/linter/php8.2.yaml) - Performs Syntax Checks of the file type `.php`.

### 🏰 - Static Tests

Static tests run against a given codebase without booting the application itself. Static tests only check the source 
code, try to find issues in it and report them. No application code is executed in this stage.

Depending on the tests that you want to run, static tests can be very fast or very slow/heavy in their execution. If
pipeline minutes are capped, you should try to add only the ones that are the most effective for you. 

- [PDepend](/static/pdepend.yaml)
- [PHP Copy/Paste Detector](/static/phpcpd.yaml)
- [PHP Mess Detector](/static/phpmd.yaml)

### 🏛 - Architecture

The Architecture stage is meant to be a stage where static tests can happen, but with a different focus. Where tools as
`phpmd` or `phpstan` check the content of given files, there are also static tool that work on a different level. 
`deptrac` for example checks how classes interact with each other and validates a given ruleset to block certain 
interactions. If, for example, you don't want to send E-Mails from your Controller, then `deptrac` can prevent such code
whereas `phpmd` couldn't care less. Think of this stage as a `We want to built software this way` stage.

- [PHP Metrics](/static/phpmetrics.yaml) - `PHP Metrics` collects a bunch of KPIs and presents them in a nice HTML UI.
- [deptrac](/architecture/deptrac.yaml) - `deptrac` validates your software architecture against a given ruleset.

### 🦄 - Automated Testing

This stage is where the magic happens. In here, your actual application should be tested. There are different types of 
automated tests. Unit- and Integration-Tests are the popular ones. We can also have Frontend Tests, A/B Tests, 
Screenshot Tests and so on. This is the stage, where the application under test is running and can be tested against.

There are pre-built templates for the following test scenarios:

- [Unit Tests]() - Runs `PHP Unit` against a given ruleset.
- [Integration Tests]() - Runs `PHP Unit` against a given ruleset. _Usually, this ruleset differs from the Unit-Test one._

### 📄 - Documentation

Many project lack when it comes to documentation. If documentation is part of your codebase, developers usually have a
better time actually writing it. This stage is meant for two things. Firstly, render given files into a documentation 
and secondly checking the content of given files. 

### 🧱 - Pre Deployment

Deployments can be complex and might involve manual tasks to execute before the code actually gets pushed to the server.
Maybe you want to pre-build/compile/transpile the whole codebase and push an artefact. Maybe you want to do a database
backup beforehand. All these things belong in this stage.

### 🚀 - Deployment

The Deployment stage is meant for a single or multiple deployment steps. Upload code to your server, restart specific
services or clear a CDNs cache.

### ☕️ - Post Deployment

After a Deployment is done, there are often cases where you want to test something again the deployed system. Perform a 
security test, generate the latest Sitespeed data, fire some smoke tests or inform other systems that a new version is 
available.

![Pipeline Preview](/assets/PipelinePreview.png)

## Adding a Step to your GitLab Pipeline

Use the following snippet to add xml linting to your project:

```yaml
stages:
  - lint

include:
  - remote: 'https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/linter/xml.yaml'
```

## Configure a Step

Initially the steps were created for Laravel and Magento 2 Projects, so most of the default values in the `VARIABLES` are preconfigured for either Laravel or Magento 2. 
It will be necessary for some cases to configure the step with variables. 

The following example shows how you can configure the Smoke Test made with [Cigar](https://github.com/Brunty/cigar): 

```yaml
variables:
  SMOKE_TEST_BASE_URL: "https://example.com/overwritten"

stages:
  - post-deploy

include:
  - remote: 'https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/test/smoke.yaml'
```
 
## Step Overview

All the steps that are included in this repository are supposed to be plug and play steps, so no additional config should be required. Of course, some checks do more than you might want. If so, you can configure them via environment variables. 

### Magento 2 Integrity Checker

This step is a static test to find all missing dependency declarations in the `composer.json` and `module.xml` files of your code.

```yaml
include:
  - remote: 'https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/magento2/integrity-checker.yml'
```

### Deployment Sentry Release

This step is a deployment step that creates release and deployment in Sentry service and sends release's commits information.

```yaml
include:
  - remote: 'https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/deployment/sentry.yml'
```

### Magento 2 Integration Tests

This step executes Magento Integration Tests based on pre-configured tests suite.

#### How to set up Integration Tests on a project

1. Include yaml config:
```yaml
include:
  - remote: 'https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/magento2/integration-tests/tests-integration.yml'
```
2. Make sure you have stage `"🦄 - Unit-/Integration Tests"` setup and running in your project pipelines;
3. Configure phpunit:
   1. Create `phpunit-integration.xml` config in project root;
   2. Use as template `https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/magento2/integration-tests/phpunit-integration.xml.dist`;
   3. Update testsuites based on your needs - see `{{PROJECT_NAME}}` and `{{PROJECT_SOURCE_FOLDER}}` placeholders.

