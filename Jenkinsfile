stage('Build Docker Image') {
    steps {
        script {
            sh "docker build -t chbaycha/devops-project:latest ."
        }
    }
}

stage('Push Docker Image') {
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push chbaycha/devops-project:latest
                """
            }
        }
    }
}
