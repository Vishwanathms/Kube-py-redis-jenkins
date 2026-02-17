pipeline {
  agent {label 'vishwa-kube'}
  triggers {
    pollSCM('H/2 * * * *')
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Check the docker cli') {
      steps {
        sh "docker --version"
        sh "docker ps -a"
      }
    }
    // stage('Build Docker Image') {
    //   steps {
    //     script {
    //       def dockerImage = docker.build("vishwa-kube/agent:${env.BUILD_ID}")
    //     }
    //   }
    // }

    // stage('Push Docker Image') {
    //   steps {
    //     script {
    //       docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
    //         dockerImage.push()
    //       }
    //     }
    //   }
    // }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
          kubectl apply -f .
          kubectl get pods
          kubectl get svc
          kubectl get deployments
        '''
      }
    }

    stage('Application Health Check') {
      steps {
        sh '''
          sleep 10
          curl http://192.168.49.2:30110
        '''
      }
    }
  }
}
