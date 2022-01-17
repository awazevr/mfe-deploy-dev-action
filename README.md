# mfe-deploy-dev-action
This is a GitHub Action meant to be used as a [composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) within an existing workflow. This action encapsulates setting up a downloads the artifacts from the build process and publishes to S3 in one step. 

The action encapsulates the following other actions:

- [benjlevesque/short-sha](https://github.com/benjlevesque/short-sha)
- [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)
- [actions/download-artifact](https://github.com/actions/download-artifact)



## Inputs

### `aws-access-key-id`

**Required** The associated AWS Secret Key Id for a given AWS Account.

### `aws-secret-access-key`

**Required** The associated AWS Secret Access Key for a given AWS Account. 

### `submodules_pat_token`

**Required** The PAT Access token required to access the submodules repository.

### `aws-region`

**Required** Specifies the AWS Region to use.

### `new-relic-api-key`

**Required** Specifies the New Relic API Key to use.

### `new-relic-region`

**Required** Specifies the New Relic Region to use.

### `new-relic-application-id`

**Required** Specifies the New Relic Application Id to use.

### `new-relic-account-id`

**Required** Specifies the New Relic account to use.

### `deployment-environment`

**Required** Specifies the environment to deploy, e.g. dev, pprd or prd




## Usage
You can use this composite Action in your own workflow by adding:

```yml
name: search-mfe-composite

on:
  push:
    branches: [ main ]

  workflow_dispatch:

jobs:
  deploy_dev:
    environment: feature
    runs-on: ubuntu-latest
    needs: [build_and_push_image, publish_static_assets]
    steps:
      - name: Deploy to Dev Environment
        uses: awazevr/mfe-deploy-dev-action@v1.0.0
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-west-2
          submodules-pat-token: ${{ secrets.SUBMODULE_PAT_TOKEN }}
          new-relic-api-key: ${{ secrets.NEW_RELIC_API_KEY }}
          new-relic-account-id: ${{ secrets.NEW_RELIC_ACCOUNT_ID }}
          new-relic-application-id: ${{ secrets.NEW_RELIC_APPLICATION_ID }}
          new-relic-region: ${{ secrets.NEW_RELIC_REGION }}
          deployment-environment




```

