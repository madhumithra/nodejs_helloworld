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
  agent any
  stages {
    stage('Build ') {
      steps {
        script{
                    bat 'npm install'
                }
        
      }
    }
      stage('Test'){
      steps{
        script{
          bat 'npm run test'
        }
      }
    }
    stage('sonar'){
      steps{
        script{
          bat 'npm install -D sonarqube-scanner'
          bat 'sonar-scanner -Dsonar.host.url=https://localhost:9000 -Dsonar.login=admin'
        }
      }
    }
    
  }
}
