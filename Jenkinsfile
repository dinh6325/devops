
pipeline {
    agent any

    tools {
        nodejs 'NodeJS-22'
    }

    environment {
        REPOSITORY = 'dinh6325/devops'
        BRANCH = 'main'
        VERCEL_URL = 'https://devops-sigma-seven.vercel.app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Telegram - Start') {
            steps {
                withCredentials([
                    string(credentialsId: 'telegram-bot-token', variable: 'TELEGRAM_TOKEN'),
                    string(credentialsId: 'telegram-chat-id', variable: 'TELEGRAM_CHAT_ID')
                ]) {
                    sh '''
                        COMMIT=$(git rev-parse --short HEAD)

                        MESSAGE="🚀 Bắt đầu deploy website
Repository: ${REPOSITORY}
Branch: ${BRANCH}
Commit: ${COMMIT}"

                        curl -s -X POST \
                            "https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage" \
                            -d chat_id="${TELEGRAM_CHAT_ID}" \
                            --data-urlencode text="${MESSAGE}"
                    '''
                }
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

        stage('Telegram - Success') {
            steps {
                withCredentials([
                    string(credentialsId: 'telegram-bot-token', variable: 'TELEGRAM_TOKEN'),
                    string(credentialsId: 'telegram-chat-id', variable: 'TELEGRAM_CHAT_ID')
                ]) {
                    sh '''
                        MESSAGE="✅ Deploy thành công
Repository: ${REPOSITORY}
Branch: ${BRANCH}
Website: ${VERCEL_URL}"

                        curl -s -X POST \
                            "https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage" \
                            -d chat_id="${TELEGRAM_CHAT_ID}" \
                            --data-urlencode text="${MESSAGE}"
                    '''
                }
            }
        }
    }

    post {
        failure {
            withCredentials([
                string(credentialsId: 'telegram-bot-token', variable: 'TELEGRAM_TOKEN'),
                string(credentialsId: 'telegram-chat-id', variable: 'TELEGRAM_CHAT_ID')
            ]) {
                sh '''
                    COMMIT=$(git rev-parse --short HEAD)

                    MESSAGE="❌ Deploy thất bại
Repository: ${REPOSITORY}
Branch: ${BRANCH}
Commit: ${COMMIT}
Error: Jenkins Pipeline Failed"

                    curl -s -X POST \
                        "https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage" \
                        -d chat_id="${TELEGRAM_CHAT_ID}" \
                        --data-urlencode text="${MESSAGE}"
                '''
            }
        }
    }
}

