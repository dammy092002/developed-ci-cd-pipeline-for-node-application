# Jenkins CI/CD Pipeline for a node application

This project involves working with the following tools and cloud native tools:

* Working in AWS
* Using Jenkins to implement Continuous Integration and Continuous Deployment
* Building pipelines
* Working with CloudFormation to deploy clusters
* Building Kubernetes clusters
* Building Docker containers in pipelines

## Project Description.
This project involves developing a Jenkins CI/CD pipeline for a simple node application using rolling deployment strategy.
The CI pipeline includes the following stages:
* Cloning the application code from a Git repository. Github was used as the repo. https://github.com/dammy092002/udacity
* Linting tests. Here hadolint was used to check and verify the dockerfile.
* Building the docker container image:

The CD pipeline includes the following stages:
* Pushing the container to the docker repository. Dockerhub was used as the repo. https://hub.docker.com/repository/docker/dammy092002/project
* Deploying the container to the kubernetes cluster (EKS) on AWS.
* Removing docker images not used.

The complete pipeline is well described in the attached Jenkinsfile.

## The environment setup:
There are 2 environments essential for this setup.

(1) Jenkins
Jenkins and all required plugins are installed on ubuntu VM in AWS.

(2) AWS
The AWS Environment was set up using AWS CloudFormation scripts. This is divided into 3 steps

(i) Building the base AWS Infrastructure which consists of VPC, Networks, etc. The scripts used are myinfrac.yml and infra-parameter-file.json. Run the commands below to set up the Infrastructure
./create.sh infra myinfrac.yml infra-parameter-file.json

(ii) Building the Kubernetes cluster. The scripts used are eks-cluster.yml and eks-cluster-parameter-file.json. Run the commands below to set up the Kubernetes cluster master node on AWS
aws cloudformation create-stack --stack-name eks-cluster --template-body file://eks-cluster.yml --parameters file://eks-cluster-parameter-file.json --capabilities CAPABILITY_IAM --region=us-west-2

(iii) Building the Kubernetes cluster worker nodes. The scripts used are eks-nodes.yml and eks-nodes-parameter-file.json. Run the commands below to create and join the worker nodes to the master k8s node.
aws cloudformation create-stack --stack-name eks-nodes --template-body file://eks-nodes.yml --parameters file://eks-nodes-parameter-file.json --capabilities CAPABILITY_IAM --region=us-west-2

NB: Before the 3rd step of creating the worker nodes, it is required to update the eks-nodes-parameter-file.json with the name of the kubernetes cluster as seen in the output section of the 2nd step creation stack.


## Pipeline
The CI and CD pipelines are defined in the attached Jenkinsfile. Note that the Kubernetes environment must be ready before running the Jenkins Pipeline as it is the target environment for the Jenkins tool. 

















