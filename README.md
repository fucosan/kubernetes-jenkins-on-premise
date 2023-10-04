# Kubernetes Manifests for Jenkins Deployment on kubernetes (on-premise)

Jenkins is automation tool that we use to manage kubernetes cluster.
The steps that we are use to install jenkins here is almost simmilar with explaination on jenkins official [https://www.jenkins.io/doc/book/installing/kubernetes/](documentation)

To have jenkins running on kubernetes pod make sure kubernetes is already setup. we can check kubctl with command:

``` sh
kubctl --version
```

clone this repository

``` sh
git clone git@github.com:fucosan/kubernetes-jenkins-on-premise.git
```

On deployment.yml we use custom docker image from https://hub.docker.com/r/kamalakb/jenkins-with-awscli-docker-kubectl
on this docker image we install awscli, docker and kubctl. For any reason there is a problem with docker repository,
Images can also easly build with Dockerfile from this repository, give it a tag and refer this docker on deployment.yml line 22


``` yaml
      .....
      containers:
        - name: jenkins
          image: kamalakb/jenkins-with-awscli-docker-kubectl:latest
          resources:
            limits:
              memory: "2Gi"
              cpu: "1000m"
            requests:
              memory: "500Mi"
              cpu: "500m"
         .....
```

This is is step by step to run this jenkins. 
Complete explanation can be found on jenkins official documentation

``` sh
kubectl create namespace devops-tools
kubectl apply -f serviceAccount.yaml
kubectl create -f volume.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Verify jenkins is running. run command above and wait for the jenkins pod status is running.

``` sh
kubectl get pods --namespace=devops-tools
```

Then we can visit http://<node-ip>:32000 to verify if jenkins is already running


