pipeline {
    agent any

    environment {
        QUOTES_IMAGE = 'quotes_service'
        API_IMAGE = 'api_gateway'
        FRONTEND_IMAGE = 'frontend_application'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Build and Push QuoteService') {
            steps {
                script {
                    dir('QuoteService') {
                        // Build Docker image for QuoteService
                        sh 'docker build -t $QUOTES_IMAGE -f app.dockerfile .'
                    }
                }
            }
        }

        stage('Build and Push ApiGateway') {
            steps {
                script {
                    dir('ApiGateway') {
                        // Build Docker image for ApiGateway
                        sh 'docker build -t $API_IMAGE -f app.dockerfile .'
                    }
                }
            }
        }

        stage('Build and Push FrontendApplication') {
            steps {
                script {
                    dir('FrontendApplication') {
                        // Build Docker image for FrontendApplication
                        sh 'docker build -t $FRONTEND_IMAGE -f app.dockerfile .'
                    }
                }
            }
        }

        stage('Run Docker Compose') {
            steps {
                script {
                    // Run the entire application using Docker Compose
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker containers and images after pipeline completes
                sh 'docker-compose down'
                sh 'docker rmi -f $QUOTES_IMAGE $API_IMAGE $FRONTEND_IMAGE || true'
            }
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
