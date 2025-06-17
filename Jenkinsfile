pipeline {
    agent any

    stages {
        // Define the stages of the pipeline
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
          steps {
            sh '''
                echo "Running tests..."
                test -f build/index.html
                npm run test
            '''
          }
        }

          stage('e2e Test') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                    //args '-u root:root' // Run as root to avoid permission issues
                }
            }
          steps {
            sh '''
                echo "Running e2e tests..."
                npm install serve
                node_modules/.bin/serve -s build &
                sleep 10
                npx playwright test
            '''
          }
        }

      
    }

      post {
          always {
            junit 'test-results/junit.xml'
          }
        }
}