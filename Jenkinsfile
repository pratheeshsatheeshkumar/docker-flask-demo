pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB-CRED')
    COMMIT_ID = "${GIT_COMMIT}"
        
    }
    stages { 

        stage('Build docker image and tag to latest') {
            steps {  
                sh "docker build -t pratheeshsatheeshkumar/simpleflaskapp: ${env.COMMIT_ID} ."
                sh "docker tag pratheeshsatheeshkumar/simpleflaskapp:${env.COMMIT_ID} pratheeshsatheeshkumar/simpleflaskapp:latest'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh "docker push pratheeshsatheeshkumar/simpleflaskapp:${env.COMMIT_ID}"
                sh 'docker push pratheeshsatheeshkumar/simpleflaskapp:latest' 
            }
        }
         stage('Run image') {
            steps{
                script {
                    def containerId = sh(script: "docker container ls -q", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker container stop $containerId"
            }
        }
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
