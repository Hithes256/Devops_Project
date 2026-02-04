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

        stage('Deploy Application') {
            steps {
                bat '''
                docker stop student-app || exit 0
                docker rm student-app || exit 0
                docker run -d -p 9090:80 --name student-app student-app:latest
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful"
        }
        failure {
            echo "❌ Build Failed"
        }
    }
}
