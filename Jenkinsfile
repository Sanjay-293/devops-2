pipeline {
    agent any
    
    environment {
        DOCKER_COMPOSE_VERSION = '2.17.2'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Deploy') {
            steps {
                script {
                    sh 'docker-compose down || true'
                    
                    sh 'docker-compose build'
                    sh 'docker-compose up -d'
                    
                    sh 'docker-compose exec -T web python manage.py migrate'
                }
            }
        }
        
        stage('Health Check') {
            steps {
                script {
                    sh 'sleep 10'
                    
                    sh 'curl http://localhost:8000 || exit 1'
                }
            }
        }
    }
    
    post {
        failure {
          sh 'docker-compose down'
        }
    }
}
