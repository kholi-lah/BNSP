pipeline {
    agent any

    environment {
        IMAGE_NAME = "ujiweb"
        CONTAINER_NAME = "ujiweb-container"
        PORT = "8082"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kholi-lah/BNSP.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running basic test..."'
                sh 'test -f index.html'
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p ${PORT}:80 ${IMAGE_NAME}
                """
            }
        }
    }

    post {
        success {
            echo "Deployment Success! App running at port ${PORT}"
        }
        failure {
            echo "Deployment Failed. Check logs!"
        }
    }
}
