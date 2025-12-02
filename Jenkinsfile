pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/salimmahdi/DEVOPS.git'
            }
        }
        
        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    if (fileExists('Dockerfile')) {
                        sh 'docker build -t chbaycha/devops-project:${BUILD_NUMBER} .'
                        sh 'docker tag chbaycha/devops-project:${BUILD_NUMBER} chbaycha/devops-project:latest'
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    if (fileExists('Dockerfile')) {
                        withCredentials([usernamePassword(
                            credentialsId: 'docker-jenkins',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )]) {
                            sh '''
                                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                                docker push chbaycha/devops-project:${BUILD_NUMBER}
                                docker push chbaycha/devops-project:latest
                            '''
                        }
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline terminé'
        }
        success {
            echo '✅ Pipeline terminé avec succès !'
        }
        failure {
            echo '❌ Pipeline échoué.'
        }
    }
}