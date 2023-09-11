//Testing

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
                sh 'docker build -t chaitanyajarajapu/numberic-app:""$GIT_COMMIT"" .'
                sh 'docker push chaitanyajarajapu/numberic-app:""$GIT_COMMIT""'
          }
        }
      }
    }
  }
}