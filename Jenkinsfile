pipeline{
  environment {
    DOCKER_URL = "madhupixiee/nodejs-helloworld"
    REGISTRY_NAME = "registry.me"
    DOCKER_CREDENTIAL_ID = 'dockerhub'
    DOCKER_IMAGE = "nodejs_helloworld"
    DOCKER_TAG="3.1"
    
    
  
  }
  
  agent{
        kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: jnlp
    image: 'jenkins/jnlp-slave:4.3-4-alpine'
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    args: ["--dockerfile=/workspace/dockerfile",
            "--context=dir://workspace",
            "--destination=madhupixiee/nodejs_helloworld"] # replace with your dockerhub account
    volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
      - name: dockerfile-storage
        mountPath: /workspace
  restartPolicy: Never
  volumes:
    - name: kaniko-secret
      secret:
        secretName: regcred
        items:
          - key: .dockerconfigjson
            path: config.json
    - name: dockerfile-storage
      persistentVolumeClaim:
        claimName: dockerfile-claim
"""
        }
    }
    stages {
        stage('Build'){
            steps{
                script{
                    sh 'npm install'
                }
            }
        }
       
      
        stage('Deploying into k8s'){
            steps{
                container('kaniko') {
                    /* Kaniko uses secret 'regsecret' declared in the POD to authenticate to the registry and push the image */
                    sh 'pwd && ls -l && df -h && cat /kaniko/.docker/config.json && /kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=${REGISTRY_NAME}/${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
    }
}
