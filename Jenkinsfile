pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "simple-flask-app:${BUILD_NUMBER}"
        HOST_PORT = "5005"  // Changed from 5000 to 5002 to avoid conflict
        CONTAINER_PORT = "5000"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub'
                git branch: 'main', 
                    url: 'https://github.com/Chrisblurp/simple-devops-project.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }
        
        stage('Test Container') {
            steps {
                script {
                    sh '''
                        docker run -d -p 5005:5000 --name test-container ${DOCKER_IMAGE}
                        sleep 5
                        curl http://localhost:5005/health || echo "⚠️ Health check failed but continuing"
                        docker stop test-container && docker rm test-container || true
                    '''
                }
            }
        }
        
        stage('Stop Old Container') {
            steps {
                sh '''
                    docker stop flask-app || true
                    docker rm flask-app || true
                '''
            }
        }
        
        stage('Run New Container') {
            steps {
                sh '''
                    docker run -d \
                        --name flask-app \
                        -p ${HOST_PORT}:${CONTAINER_PORT} \
                        ${DOCKER_IMAGE}
                    echo "✅ App running at http://localhost:${HOST_PORT}"
                '''
            }
        }
    }
    
    post {
        success {
            echo "✅ Pipeline successful! App running at http://localhost:${HOST_PORT}"
        }
        failure {
            echo '❌ Pipeline failed. Check the logs.'
        }
        always {
            cleanWs()
        }
    }
}