# BWCE CI/CD Demo

As part of the **TIBCO CI/CD Demo in Box** this repo containes very simple REST API application built with *TIBCO BusinessWorks Container Edition*.
Along with the BWCE files the needed artifacts for creating a proper Docker image for the application as well as the a sample for runninhg the application on Kubernetes are included.

## Required Automation Artefacts

### BWCE Application Build

An automated packaging of all BW and Java artefacts as BW application EAR a *build description* for Maven must be provided. TIBCO offers a Maven plugin for *Studio for BusinessWorks* to support creation of the appropriate pom files.

### Jenkinsfile

The demo uses Jenkins with BlueOcean plugin to automate the (build and deployment) pipeline on Jenkins. BlueOcean uses a Jenkinsfile that specifies the pipeline steps needed for the project. So, a Jenkinsfile was added to the git repository to allow for those automated builds.

### Dockerfile

In order to package the resulting application as Docker image a build description in form of the *Dockerfile* is required.


## How to check auto created docker images?

To check if the build pipline worked as expected one can first check the docker build environment. The local docker registry should include a docker image with the respective version just built. As second step this image must have been pushed to the Docker repository used by Rancher/K3s to start the application. As we are using the simple Docker registry implementation provided by docker we need to use the API to get the details on what's available.

```
# check what images are in the local docker registry
docker image ls
REPOSITORY                                   TAG              IMAGE ID       CREATED          SIZE
k3d-myregistry.localhost:12345/helloworld    1.0.3            401de8c69913   20 minutes ago   357MB
k3d-myregistry.localhost:12345/helloworld    1.0.2            0d98608c0dee   33 minutes ago   357MB
k3d-myregistry.localhost:12345/bwce-mon      2.7.2            f0eae05d5e8e   4 days ago       152MB
gcr.io/gice-293818/bwce-base-prom            2.6.2            19120ce16999   10 months ago    357MB
registry                                     2                b2cb11db9d3d   10 months ago    26.2MB
rancher/k3d-proxy                            4.4.8            1811184140a4   11 months ago    44.7MB
gcr.io/gice-293818/flogo-hello               1.0.0            9c4af3a62c69   11 months ago    29.4MB
k3d-myregistry.localhost:12345/flogo-hello   1.0.0            9c4af3a62c69   11 months ago    29.4MB
rancher/k3s                                  v1.21.3-k3s1     46a3f42c9715   12 months ago    173MB
tibco/bwce-base                              2.6.2            e846d526295d   12 months ago    357MB
node                                         10.15.3-alpine   56bc3a1ed035   3 years ago      71MB

# get info from remot docker registry on what was pushed
curl -s http://localhost:12345/v2/_catalog | jq "."
{
  "repositories": [
    "bwce-mon",
    "flogo-hello",
    "helloworld"
  ]
}

curl -s http://localhost:12345/v2/helloworld/tags/list | jq "."
{
  "name": "helloworld",
  "tags": [
    "1.0.3",
    "1.0.1",
    "1.0.2",
    "latest"
  ]
}
```

For moore details on the docker rgistry API see [Docker Registry HTTP API V2](https://docs.docker.com/registry/spec/api/).

## How to Run an BW Application as Container?

On a Kubernetes runtime a description for a POD or better a deployment is required to instruct Kubernetes on how to host the application (which images, ports, pod count etc). Therefore an application manifest YAML file is also part of the project to allow for an atomated deployment right after building a new application image.
