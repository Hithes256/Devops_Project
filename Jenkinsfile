pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Hithes256/Devops_Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t student-app:latest .'
            }
        }

        stage('Run Container (GREEN)') {
            steps {
                bat '''
                docker stop student-green || exit 0
                docker rm student-green || exit 0
                docker run -d -p 8080:80 --name student-green student-app:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                echo "If container runs, deployment is SUCCESS"
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed – rollback simulated (BLUE)"
        }
    }
}
