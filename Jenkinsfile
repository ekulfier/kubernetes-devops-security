pipeline {
  agent any

  stages {
    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }

    stage('Docker Build and Push') {
      steps {
        docker.withRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t thecodingadventure/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push thecodingadventure/numeric-app:""$GIT_COMMIT""'
        }
        // withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'DOCKER_REGISTRY_PWD', usernameVariable: 'DOCKER_REGISTRY_USER')]) {
        //   sh 'printenv'
        //   sh 'docker build -t thecodingadventure/numeric-app:""$GIT_COMMIT"" .'
        //   sh 'docker push thecodingadventure/numeric-app:""$GIT_COMMIT""'
        // }
      }
    }
  }
}