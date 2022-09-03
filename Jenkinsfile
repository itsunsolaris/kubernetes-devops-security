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
              docker.withRegistry([credentialsId, "docker-hub", url: ""]) {
                sh 'printenv'
                sh 'docker build -t imranmalikit/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push imranmalikit/numeric-app:""$GIT_COMMIT""'
                sh "mvn test"
            }
        }                     
    }
}

}