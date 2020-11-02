# Challenge 1
Build a Jenkins CI/CD pipeline for a Dockerized Spring/Node/DotNet microservice on a Kubernetes environment on AWS using Jenkins. Set up your cloud infrastructure using `awscli`, register a GitHub hook to trigger a Jenkins build, and validate code quality with SonarCloud before publishing a Docker image to DockerHub. Use `kubectl` to deploy the image to both a testing and staging environment. Report build, test, and deployment results via Slack/Discord or email message.

## AWS Setup
Setup AWS infrastructure using `awscli`
Make sure that you have aready configurated your credentials is '~/.aws/credentials', this is user specific.

If not, follow the AWS official documentation here https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html

The easiest way to create the EKS would be using 'ekscli'

'$ chocolatey install -y eksctl' 

'$ eksctl version'

```
$ eksctl create cluster \
    --name devops-training \
    --version 1.17 \
    --region us-west-1 \
    --nodegroup-name devnodes \
    --node-type t2.medium\
    --nodes 2
```

Now this will take 15+ minutes. Make sure to change your K8S context to the new Kubernetes you created.

## Dockerhub
Building your custom Jenkins and uploading it to dockerhub. Make sure you are logged into your DockerHub before pushing. 

If Dockerhub is not yet configured, refer to Docker documentation here https://docs.docker.com/engine/reference/commandline/login/

'$ docker build -t yourregistry/jenkinsimage jenkins/'

'$ docker push yourregistry/jenkinsimage:latest'

This will also take a while, depending on your internet speed.