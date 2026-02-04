pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *')   // checks GitHub every 1 minute
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Hithes256/Devops_Project.git',
                    credentialsId: 'github-token'

            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t student-app:latest .'
            }
        }

        stage('Deploy GREEN') {
            steps {
                bat '''
                docker rm -f green-app || exit 0
                docker run -d -p 9090:80 --name green-app student-app:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                bat '''
                timeout /t 5
                curl http://localhost:9090 || exit 1
                '''
            }
        }

        stage('Switch Traffic') {
            steps {
                bat '''
                docker rm -f blue-app || exit 0
                docker rename green-app blue-app
                '''
            }
        }
    }

    post {
        failure {
            bat '''
            echo "❌ Deployment failed. Rolling back..."
            docker rm -f green-app || exit 0
            '''
        }
    }
}
