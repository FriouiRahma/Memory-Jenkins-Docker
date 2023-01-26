pipeline {
    agent any
     environment {
            DOCKER_CERT_PATH = credentials('docker')
        }
    stages {
        stage('Building docker image client') {
            steps {
                dir("client/"){
                    bat 'docker build -t rahmafrioui/client .'
                       }  
            }
        }
      stage('Login to DockerHub') {
      steps {
          sh 'docker login -u $DOCKER_CREDS_USR -p $DOCKER_CREDS_PSW'
        }
      }
    
       stage('Push to DockerHub front') {
            steps {
                sh 'docker push $DOCKER_CREDS_USR/client'
      }
    }
    }
}
