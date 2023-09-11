pipeline {
    agent {
        kubernetes{
            inheritFrom 'cd-k8s-1-25-builder'
            defaultContainer 'golang'
        }
    }

  stages{
   stage('Build Artifact') {
            steps {
              //container('maven'){
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