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
 
