# Gitlab Pipeline Templates

This Repository is a collection of useful reusable GitLab-Ci Step-Templates. No need to add the same CI-Steps into your projects over and over again. Simply use the [GitLab Include Feature](https://docs.gitlab.com/ee/ci/yaml/README.html#include) and the
steps that you think are necessary for your specific project. 

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

