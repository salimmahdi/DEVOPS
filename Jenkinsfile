pipeline {
    agent any

    tools {
        maven 'Maven3'  // À configurer dans Jenkins
        jdk 'JDK17'     // À configurer dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/salimmahdi/mon-project.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean package'  // Inclut compile, test, package
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Vérifie si Dockerfile existe
                    if (fileExists('Dockerfile')) {
                        sh 'docker build -t mon-project:${BUILD_NUMBER} .'
                        sh 'docker tag mon-project:${BUILD_NUMBER} mon-project:latest'
                    } else {
                        echo 'Pas de Dockerfile trouvé, étape Docker ignorée'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            when {
                expression { fileExists('Dockerfile') }
            }
            steps {
                script {
                    // Nécessite credentials Docker Hub dans Jenkins
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh '''
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push mon-project:${BUILD_NUMBER}
                            docker push mon-project:latest
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé'
            // Nettoyage
            sh 'docker system prune -f || true'
        }
        success {
            echo '✅ Pipeline terminé avec succès !'
            // Ici vous pourriez ajouter des notifications
        }
        failure {
            echo '❌ Pipeline échoué.'
        }
    }
}