pipeline {
    agent any
    
    environment {
        APP_NAME = "simple-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_PORT = "5000"
        HOST_PORT = "5000"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Nnamdijohn027/simple-devops-project.git',
                    credentialsId: 'GitHub-credentials'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${APP_NAME}:${IMAGE_TAG}")
                }
            }
        }
        
        stage('Test Image') {
            steps {
                script {
                    // Run container and test
                    sh """
                        docker run -d --name test-${APP_NAME} -p 5001:5000 ${APP_NAME}:${IMAGE_TAG}
                        sleep 5
                        curl -f http://localhost:5001/health                         c         dock          st-${APP_NAME}
                                       ${APP                                                                                           tage('    oy') {
            s            s          script {
                                                   # Stop and remove old container if exists
                        docker stop ${APP_NAME} || true
                        docker rm ${APP_NAME} || true
                        
                        
ocker rm ${ntocker rm ${nt       ocker rm ${ntoer run -d \
                          --name ${APP_NAME} \
                          -p ${HOST_PORT}:${CONTAINER_PORT} \
                          ${APP_               AG}
                                                                      ye                      "
                    """
                }
            }
        }
    }
    
    post {
        always {
            echo "Pipeline com            echo "Pipeline com            echo "Pipeline com            echo "Pipeline com            echo "Pipeline com  n ${IMAGE_TAG}"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
