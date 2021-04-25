pipeline{
  environment {
    registry = "madhupixiee/nodejs-helloworld"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  
  agent{
    kubernetes {
            
            label 'test'
            defaultContainer 'jnlp'
            yamlFile 'pod.yaml'
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
        stage('Build:docker') {
            steps{
                container('kaniko') {
                    /* Kaniko uses secret 'regsecret' declared in the POD to authenticate to the registry and push the image */
                    sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=registry.me:5000/nodejs_helloworld:3.5'
                }
             }
          }
          
    }
}
