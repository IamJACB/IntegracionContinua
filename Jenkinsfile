pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        DOCKER_HOST = "unix:///var/run/docker.sock"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AndresObandoPDLT/IntegracionContinua.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Run Containers') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'curl -f http://backend:3005 || exit 1'
            }
        }

        stage('Finish') {
            steps {
                echo 'CI Pipeline completed successfully.'
            }
        }
    }
}