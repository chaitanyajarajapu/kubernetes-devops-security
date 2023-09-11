pipeline {
    agent {
        kubernetes{
            inheritFrom 'cd-k8s-1-25-builder'
            //defaultContainer 'builder'
        }
    }

  stages{
   stage('Build Artifact') {
            steps {
              container('builder'){
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
              }
          }
   }

      stage('Unit Test') {
            steps {
              sh "mvn test"
            }
      }
  }
}