pipeline {
    agent any
    environment {
        // Define your registry and credentials ID from Jenkins
        DOCKER_IMAGE = 'your-username/my-app'
        REGISTRY_CREDS = 'docker-hub-credentials'
    }
    stages {
        stage('Build Image') {
            steps {
                script {
                    // Builds the image and tags it with the Jenkins build number
                    app = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Push to Registry') {
            steps {
                script {
                    // Log in and push using Jenkins Credentials
                    docker.withRegistry('', REGISTRY_CREDS) {
                        app.push()
                        app.push('latest')
                    }
                }
            }
        }
    }
}
