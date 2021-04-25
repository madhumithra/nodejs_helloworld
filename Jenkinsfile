/**
 * This pipeline will build and deploy a Docker image with Kaniko
 * https://github.com/GoogleContainerTools/kaniko
 * without needing a Docker host
 *
 * You need to create a jenkins-docker-cfg secret with your docker config
 * as described in
 * https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-in-the-cluster-that-holds-your-authorization-token
 *
 * ie.
 * kubectl create secret docker-registry regcred --docker-server=https://index.docker.io/v1/ --docker-username=csanchez --docker-password=mypassword --docker-email=john@doe.com
 */

pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      label 'test'
      defaultContainer 'jnlp'
      yaml """
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    args: ["--dockerfile=/workspace/dockerfile",
            "--context=dir://workspace",
            "--destination=madhupixiee/nodejs_helloworld"]
    imagePullPolicy: Always
    command:
    - sleep
    args:
    - 9999999
    volumeMounts:
      - name: regcred
        mountPath: /kaniko/.docker
    volumes:
    - name: regcred
    projected:
      sources:
      - secret:
          secretName: regcred
        items:
          - key: .dockerconfigjson
            path: config.json
    
"""
    }
  }
  stages {
    stage('Build with Kaniko') {
      steps {
        container('kaniko') {
                    /* Kaniko uses secret 'regsecret' declared in the POD to authenticate to the registry and push the image */
                    sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=registry.me:5000/nodejs_helloworld:5.1'
                }
        
      }
    }
  }
}
