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

      stage('Test Maven') {
            steps {
              sh "mvn clean verify sonar:sonar  -Dsonar.projectKey=Numeric-Application -Dsonar.host.url=http://imrandevops.eastus.cloudapp.azure.com:9000  -Dsonar.login=sqp_ae6342a3e7a0116f09be47f67c1d6c8fc4e6aa7f"
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