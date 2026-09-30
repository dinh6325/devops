pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
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
                    node --version
                    npm --version
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    ls -la
                    test -f index.html
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    test -f index.html
                    echo "index.html exists"
                '''
            }
        }

        stage('Install Vercel CLI') {
            steps {
                sh '''
                    npm install -g vercel
                    vercel --version
                '''
            }
        }

        stage('Deploy to Vercel') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'vercel-token',
                        variable: 'VERCEL_TOKEN'
                    )
                ]) {
                    sh '''
                        vercel --prod --yes \
                            --token "$VERCEL_TOKEN" \
                            --name devops
                    '''
                }
            }
        }
    }
}