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




## Usage
You can use this composite Action in your own workflow by adding:

```yml
name: search-mfe-composite

on:
  push:
    branches: [ feature/MFE-3 ]
  # pull_request:
  #   branches: [ master ]

  workflow_dispatch:

jobs:
  build_and_push_image:
    environment: feature
    env:
      ECR_REPOSITORY: dev-age-search-mfe
    runs-on: ubuntu-latest

    steps:
      - name: Checkout, Build and Push Docker Image
        uses: awazevr/docker-build-push-action@v1.0.20
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-west-2
          bit-token: ${{ secrets.BIT_TOKEN }}
          pat-token: ${{ secrets.PAT_TOKEN}}

```

