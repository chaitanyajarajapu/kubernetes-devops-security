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
              container('kubectl'){
              withKubeConfig([ credentialsId: "kubeconfig"]) {
                sh "sed -i 's#replace#chaitanyajarajapu/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
                //sh 'kubectl --kubeconfig config create ns devsecops'
                sh 'kubectl apply -f k8s_deployment_service.yaml -n jenkins --validate=false'
                sh 'kubectl apply -f /mysql/dep.yaml -n jenkins --validate=false'
                //sh 'kubectl --kubeconfig config create deploy node-app --image siddharth67/node-service:v1 -n devsecops'
                //sh 'kubectl --kubeconfig config expose deploy node-app --name node-service --port 5000 -n devsecops'
              }
        .yaml
      }
    }
  }
}
