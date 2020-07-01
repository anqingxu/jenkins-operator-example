# Jenkins - Webhook Relay operator example

In this example we will create a simple CI system that uses Jenkins to build our example application.

# Short intro

1. Operator works by checking a repo for specific directories in it, more info here https://jenkinsci.github.io/kubernetes-operator/docs/getting-started/latest/configuration/, but the main idea is that you have to prepare pipelines and job definition in your GitHub repository using the following structure:

  ```
  cicd/
  ├── jobs
  │   └── build.jenkins
  └── pipelines
      └── build.jenkins
  ```

2. In this repository I am going to set this up. If you are forking, then just update `cicd/jobs/build.jenkins` and `cicd/pipelines/build.jenkins` with your own fork names.

# Installation

## Jenkins operator

We begin by installing Jenkins operator:

```
helm repo add jenkins https://raw.githubusercontent.com/jenkinsci/kubernetes-operator/master/chart
helm repo update
helm install jenkins/jenkins-operator
```

Official docs can be found here: https://jenkinsci.github.io/kubernetes-operator/docs/installation/.


Forward traffic to Jenkins:

```
kubectl --namespace jenkins port-forward jenkins-example 8080:8080
```

Lookup username/password (if you have renamed CR, then your secret name will also have a different name):

```
kubectl --namespace jenkins get secret jenkins-operator-credentials-example -o 'jsonpath={.data.user}' | base64 -d
jenkins-operator                                                                                                                                      

kubectl --namespace jenkins get secret jenkins-operator-credentials-example -o 'jsonpath={.data.password}' | base64 -d
oHlaSPMslZr3w
```

And now you can open your browser:

![](static/login.png)