pipeline {
    agent any
    stages {
        stage('Start') {
            steps {
                echo 'Lab_2: started by GitHub'
            }
        }
        stage('Image build') {
            steps {
                sh "docker build -t prikm:latest ."
                
                // Тегуємо образ різними версіями
                sh "docker tag prikm твій_логін_dockerhub/prikm:latest"
                sh "docker tag prikm твій_логін_dockerhub/prikm:$BUILD_NUMBER"
                sh "docker tag prikm твій_логін_dockerhub/prikm:staging"
            }
        }
        stage('Push to registry') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_credentials", url: ""]) {
                    sh "docker push твій_логін_dockerhub/prikm:latest"
                    sh "docker push твій_логін_dockerhub/prikm:$BUILD_NUMBER"
                    sh "docker push твій_логін_dockerhub/prikm:staging" 
                }
            }
        }
        stage('Deploy image') {
            steps {
                sh "docker run -d -p 80:80 natalia/prikm"
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed, please check the logs.'
        }
    }
}

