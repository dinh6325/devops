pipeline {
    agent any

    tools {
        nodejs 'NodeJS-22'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Node') {
            steps {
                sh '''
                    echo "===== Node.js ====="
                    node --version

                    echo "===== npm ====="
                    npm --version
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Building website...'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing website...'
            }
        }
    }
}