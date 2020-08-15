
# TimeOff Management - Gorilla Logic DevOps Test

For the original TimeOff Management documentation please visit: <a href="https://github.com/timeoff-management/timeoff-management-application">https://github.com/timeoff-management/timeoff-management-application</a>


## Deployment

The suggested deployment leverages K8S and it's integration with AWS to provide deployments that are high available (across availability zones), load balanced, and cause zero-downtime.


## Environment Setup

In order to deploy the resources defined in this repo make sure to set the following tooling in your machine:

```
- Kubectl >= 1.16
- AWS CLI >= 1.17.7
```

After setting up proper credentials for the target environment, make sure to setup the K8S authentication via the AWS CLI:
```
aws eks --region us-east-1 update-kubeconfig --name default
```

# CI / CD

The working CI flow consists of:

### On every push to a Pull Request to the master branch
- Automated tests run
- Automated build of the Docker image
- Automated publish of the Docker image with a tag name after the PR number (i.e. PR-1)


### On every merge of a Pull Request to the master branch
- The Docker image is built and tagged with the branch name (this could also be the release number)
- The application is deployed to a target environment (usually dev or test) making use of Helm and templated K8S manifests. The deployment is a rolling upgrade which  only replaces running pods/instances of the application if the newly created workloads are healthy.
- (Optional) A K8S job could be run to perform the database migrations. Not included for simplicity's sake.

