# GitOps-style Continuous Delivery For Kubernetes Engine With Cloud Build

This repository contains the code used in the
[GitOps-style Continuous Delivery with Cloud Build](https://cloud.google.com/kubernetes-engine/docs/tutorials/gitops-cloud-build)
tutorial.

GitOps is a Continuous Delivery approach [first described by Weaveworks](https://www.weave.works/blog/gitops-operations-by-pull-request) that is
popular in the Kubernetes community. A key part of GitOps is the idea of
"environments-as-code": describing your deployments declaratively by files (for
example, Kubernetes manifests) stored in a Git repository.

In this tutorial, you create a CI/CD pipeline that automatically builds a
container image from commited code, stores the image in Google Container
Registry, updates a Kubernetes manifest in a Git repository and triggers a
deployment to Kubernetes Engine using that manifest.

This tutorial uses two Git repositories: one for the application —the _app_
repository— and one for storing the deployment manifests —the _env_ repository.
When a change is pushed to the application repository, tests are run, a
container image is built and pushed to Container Registry. Once the image is
pushed, the deployment manifests are updated to use that new image and they are
pushed to the _candidate_ branch of the _env_ repository. This triggers the actual
deployment in Kubernetes. Once the deployment is finished, the new manifests
are copied over to the _production_ branch of the _env_ repository.

In the end, you have a system where:
* The _candidate_ branch is a history of the deployment attempts.
* The _production_ branch is a history of the successful deployments.
* You have a view of successful and failed deployments in Cloud Build.
* You can rollback to any previous deployment by re-executing the corresponding
  job in Cloud Build. A rollback also updates the _production_ branch to
  truthfully reflect the history of deployments.
