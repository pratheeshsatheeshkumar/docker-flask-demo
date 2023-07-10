pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB-CRED')
    }
    stages { 

        stage('Build docker image and tag to latest') {
            steps {  
                sh 'docker build -t pratheeshsatheeshkumar/simpleflaskapp:$BUILD_NUMBER .'
                sh 'docker tag pratheeshsatheeshkumar/simpleflaskapp:$BUILD_NUMBER pratheeshsatheeshkumar/simpleflaskapp:latest'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push pratheeshsatheeshkumar/simpleflaskapp:$BUILD_NUMBER'
                sh 'docker push pratheeshsatheeshkumar/simpleflaskapp:latest' 
            }
        }
         stage('Run image') {
            steps{
                sh 'docker container rm $(docker container ls -qa)'
                sh 'docker run -d --rm --name $BUILD_NUMBER -p 8081:8080 pratheeshsatheeshkumar/simpleflaskapp:latest'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}
