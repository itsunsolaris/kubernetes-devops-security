pipeline {
  
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'  //so that they can be downloaded later  
            }
        }

      stage('Test Maven') {
            steps {
              sh "mvn test"
            }
        }

      stage('SonarQube analysis') {
            steps {
              withSonarQubeEnv('SonarQube') {
              sh 'mvn clean verify sonar:sonar'
              }
            }
         }

      stage('Docker Build and Push') {
            steps {
              withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                sh 'printenv'
                sh 'docker build -t imranmaikit/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push imranmaikit/numeric-app:""$GIT_COMMIT""'
            }
        }                     
    }

      stage('Kubernetes Deployment - DEV') {
            steps {
              withKubeConfig([credentialsId: 'kubeconfig']) {
                sh "sed -i 's#replace#imranmaikit/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
                sh "kubectl apply -f k8s_deployment_service.yaml"
            }
        }                     
    }    
}
}