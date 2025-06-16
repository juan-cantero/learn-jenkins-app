pipeline {
  agent any
  
  stages {
    stage('Build') {
      agent {
        docker {
          imgage 'node:18-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
          ls -la
          node --version
          npm --version
          npm install
          npm ci
          npm run build
          ls -la
        '''
      }
    }
  }
}