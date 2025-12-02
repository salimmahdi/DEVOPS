pipeline {
    agent any

    stages {

        stage('Clean & Build') {
            steps {
                bat 'mvn clean install -DskipTests -B'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t chbaycha/devops-project:latest ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                                 usernameVariable: 'DOCKER_USER',
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    bat """
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker push chbaycha/devops-project:latest
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline terminée"
        }
    }
}
