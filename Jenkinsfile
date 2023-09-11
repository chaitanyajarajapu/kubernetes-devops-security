pipeline {
    agent {
        kubernetes{
            inheritFrom 'cd-k8s-1-25-builder'
            //defaultContainer 'docker'
        }
    }

  stages{
   stage('Build Artifact') {
            steps {
              container('maven'){
              sh "mvn clean package -DskipTests=true"
              archiveArtifacts 'target/*.jar' //so that they can be downloaded later
            }
          }
        }

      stage('Unit Test') {
            steps {
              container('maven'){
              sh "mvn test"
              }
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }  
          }
      stage('Build and Push Image') {
            steps {
              container('docker'){
              withDockerRegistry([ credentialsId: "docker-hub", url: ""]) {
                sh 'printenv'
                sh 'docker build -t chaitanyajarajapu/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push chaitanyajarajapu/numeric-app:""$GIT_COMMIT""'
          }
        }
      }
    }
      stage('Kubernetes Deployment - DEV') {
            steps {
              //container('kubernetes'){
              //withKubeConfig([ credentialsId: "kubeconfig"]) {
              script{
                sh "sed -i 's#replace#chaitanyajarajapu/numeric-app:$GIT_COMMIT#g' k8s_deployment_service.yaml"
                sh 'kubectl apply --kubeconfig config -f k8s_deployment_service.yaml'
          //}
        //}
        }
      }
    }
  }
}