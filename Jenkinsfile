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
      stage('Docker Build and Push') {
            steps {
              withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                sh 'printenv'
                sh 'docker build -t imranmailkit/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push imranmailkit/numeric-app:""$GIT_COMMIT""'
                sh "mvn test"
            }
        }                     
    }
}
}