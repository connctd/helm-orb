# Helm Orb

Simple orb that lets you package helm3 charts and publish them on gcs.

## Example

This example uses the combined command helm/init-package-and-publish which installs helm and gcs plugin and also does the package and publish part. There are more atomic commands inside src/commands.

```
orbs:
  helm: connctd/helm@dev:e3cf760
[...]

jobs:
  publish-chart:
    docker:
      - image: google/cloud-sdk
    steps:
      - checkout
      - run:
          name: Define chart version based on git tag
          command: |
            echo "export CHART_VERSION=$(git describe --tags --always --dirty)" >> $BASH_ENV
      - helm/init-package-and-publish:
          service-account-credentials: $GCLOUD_SERVICE_ACCOUNT
          bucket: $HELM_BUCKET_NAME
          chart-version: $CHART_VERSION
```