# Challenge 1
Build a Jenkins CI/CD pipeline for a Dockerized Spring/Node/DotNet microservice on a Kubernetes environment on AWS using Jenkins. Set up your cloud infrastructure using `awscli`, register a GitHub hook to trigger a Jenkins build, and validate code quality with SonarCloud before publishing a Docker image to DockerHub. Use `kubectl` to deploy the image to both a testing and staging environment. Report build, test, and deployment results via Slack/Discord or email message.

# Configuration

## AWS Setup
Setup AWS infrastructure using `awscli`
Make sure that you have aready configurated your credentials is '~/.aws/credentials', this is user specific.

If not, follow the AWS official documentation here https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html

The easiest way to create the EKS would be using 'ekscli'

```
$ chocolatey install -y eksctl
```

Check if your have successfully installed the eksctl

```
$ eksctl version
```

Now you can create the cluster

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

Now you can build your image with your own username instead of 'yourregistry'

```
$ docker build -t yourregistry/jenkinsimage jenkins/
```

Now you can push up your image.
```
$ docker push yourregistry/jenkinsimage:latest
```

This will also take a while, depending on your internet speed.

## Jenkins
With your images successfully pushed and EKS context correctly configurated, now you can start your services.

```
$ kubectl apply -f jenkins/k8s.yaml -n jenkinsns
```

You can check the status of your pods by using 

```
$ kubectl get pods -o wide -n jenkinsns
```

If you need password for the, copy the password by views the logs

## Access via Browser

Now it is all setup, you can copy the external url and start building your project! Your URL can also be accessed via: 

```
$ kubectl get services -n jenkinsns
```
