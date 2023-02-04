pipeline {
    agent any
     environment {
             DOCKERHUB_CREDENTIALS = credentials('docker')
             scannerHome = tool 'SonarQube'
        }
    stages {
        stage('Building docker image client') {
            steps {
                dir("client/"){
                    bat "docker build -t $DOCKERHUB_CREDENTIALS_USR/client ."
                       }  
            }
        }
      stage('Login to DockerHub') {
      steps {
          bat "docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW"
        }
      }
    
       stage('Push to DockerHub front') {
            steps {
                bat "docker push $DOCKERHUB_CREDENTIALS_USR/client"
      }
    }

   stage('SonarQube analysis') {
            steps {
                //   dir("client/"){
                //     bat "npm run sonar"
                //   }
                  withSonarQubeEnv(installationName: 'sq1') {
                       bat "${scannerHome}/bin/sonar-scanner.bat \
                        -D sonar.login: admin \
                        -D sonar.password: admin \
                        -D sonar.projectKey=test-js \
                        -D sonar.sources=./src \
                        -D sonar.test.inclusions= **/*.test.tsx,**/*.test.ts \
                        -D sonar.projectName=test-react \
                        -D sonar.exclusions=  **/*.test.tsx \
                        -D sonar.projectVersion=1.0 \
                        -D sonar.testExecutionReportPaths=test-reporter.xml \
                        -D sonar.analysis.mode=publish \
                         -D sonar.host.url=http://localhost:9000 \
                        "
                    }    
                
            }
        }


    }
}
