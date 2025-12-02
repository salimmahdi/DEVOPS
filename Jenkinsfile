stage('Push Docker Image') {
    when {
        expression { fileExists('Dockerfile') }
    }
    steps {
        script {
            withCredentials([usernamePassword(
                credentialsId: 'docker-jenkins',  // ← ICI votre ID
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {
                sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push chbaycha/mon-project:${BUILD_NUMBER}
                    docker push chbaycha/mon-project:latest
                    echo "Image pushed successfully!"
                '''
            }
        }
    }
}