# Magento 2 - Integration Tests pipeline config docs

## How to set up Integration Tests on a project

1. Include `tests-integration.yml` config into your pipeline setup - `https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/magento2/integration-tests/tests-integration.yml`;
2. Make sure you have stage `"🦄 - Unit-/Integration Tests"` setup and running in your project pipelines;
3. Put phpunit config into the root of your project - use `https://raw.githubusercontent.com/run-as-root/gitlab-pipeline-templates/master/magento2/integration-tests/phpunit-integration.xml.dist` as a template. Update testsuites based on your needs - see `{{PROJECT_NAME}}` and `{{PROJECT_SOURCE_FOLDER}}` placeholders.

## Description of `tests-integration.yml`
