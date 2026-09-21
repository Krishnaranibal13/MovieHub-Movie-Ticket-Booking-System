pipeline {

    agent any

    environment {
        COMPOSE_PROJECT_NAME = "moviehub"
        ENV_FILE = "/home/ubuntu/MovieHub-Movie-Ticket-Booking-System/.env"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out MovieHub source code...'
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building MovieHub Docker images...'
                sh 'docker compose build'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying MovieHub application...'

                sh '''
                    if [ ! -f "$ENV_FILE" ]; then
                        echo "ERROR: .env file not found at $ENV_FILE"
                        exit 1
                    fi

                    docker compose --env-file "$ENV_FILE" up -d
                '''
            }
        }

        stage('Verify Containers') {
            steps {
                echo 'Checking container status...'
                sh 'docker compose ps'
            }
        }

        stage('Application Health Check') {
            steps {
                echo 'Checking MovieHub application...'

                sh '''
                    sleep 10
                    curl -f http://localhost/ || exit 1

                    echo "MovieHub application is running successfully."
                '''
            }
        }
    }

    post {

        success {
            echo 'MovieHub deployment completed successfully.'
        }

        failure {
            echo 'MovieHub deployment failed. Check the Jenkins console output.'
        }

        always {
            echo 'Deployment pipeline finished.'
        }
    }
}
