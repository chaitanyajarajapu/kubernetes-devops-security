pipeline {
    agent {
        kubernetes{
            inheritFrom 'cd-k8s-1-25-builder'
            //defaultContainer 'maven'
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
            post{
              always{
                junit 'target/sunfire-reports/*.xml'
                jacaco execPatterns: 'target/jacaco.exec'
              }
            }  
            }
      }
  }
}