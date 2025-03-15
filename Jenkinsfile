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
                sh "docker tag prikm natalia/prikm:latest"
                sh "docker tag prikm natalia/prikm:$BUILD_NUMBER"
                sh "docker tag prikm natalia/prikm:staging"
            }
        }
        stage('Push to registry') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_token", url: ""]) {
                    sh "docker push natalia/prikm:latest"
                    sh "docker push natalia/prikm:$BUILD_NUMBER"
                    sh "docker push natalia/prikm:staging"
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

