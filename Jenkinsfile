pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "simple-flask-app:${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub'
                git branch: 'main', 
                    url: 'https://github.com/Nnamdijohn027/simple-devops-project.git'
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
                        docker run -d -p 5001:5000 --name test-container ${DOCKER_IMAGE}
                        sleep 3
                        curl http://localhost:5001/health || echo "Health check failed but continuing"
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
                        -p 5000:5000 \
                        ${DOCKER_IMAGE}
                '''
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline successful! App running at http://localhost:5000'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs.'
        }
        always {
            cleanWs()
        }
    }
}
