pipeline {
  agent any

  environment {
    IMAGE_NAME = "trend-store"
    DOCKERHUB_USER = "manasadevi09"
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/manasamantripragada-oss/Trend-Store.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t manasadevi09/trend-store:latest .'
      }
    }

    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh '''
            echo $PASS | docker login -u $USER --password-stdin
            docker push manasadevi09/trend-store:latest
          '''
        }
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh '''
          kubectl apply -f deployment.yml
          kubectl apply -f service.yml
        '''
      }
    }
  }
}

