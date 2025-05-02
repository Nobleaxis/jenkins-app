pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                ls -al
                node --version
                npm --version
                npm ci
                npm run build
                ls -al
                '''
            }
        }

        stage('Test') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                echo 'this is the the testing stage'
                test -f build/index.html
                npm test
                '''
            }
        }

        stage('E2E Testing') {
            agent {
                docker{
                    image 'mcr.microsoft.com/playwright:v1.52.0-noble'
                    reuseNode true
                }
            }
            steps{
                sh '''
                 npm install -g serve
                 serve -s build
                 npx playwright test
                '''
            }
        }
    }

    post {
        always{
            junit 'test-results/junit.xml'
        }
    }
}
