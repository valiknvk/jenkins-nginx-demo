pipeline {
    agent any
    parameters {
        string(name: 'APP_PORT', defaultValue: '', description: 'Override port (optional)')
    }
    environment {
        APP_NAME = 'jenkins-nginx-demo'
        APP_PORT = "${params.APP_PORT ?: '8081'}"
    }

    stages {
        stage('Inspect') {
            steps {
                sh '''
                pwd
                ls -la
                '''
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t $APP_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f $APP_NAME || true
                docker run -d -p $APP_PORT:80 --name $APP_NAME $APP_NAME
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh 'curl -f http://localhost:$APP_PORT'
            }
        }
    }

    post {
        success {
            echo 'Deploy OK!!!'
        }
        failure {
            echo 'Deploy FAILED'
        }
    }
}
