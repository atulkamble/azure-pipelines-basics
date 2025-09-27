pipeline {
  agent any

  options {
    timestamps()
    ansiColor('xterm')
  }

  environment {
    NODE_VERSION = '18'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Setup Node') {
      steps {
        sh '''
          if ! command -v n &> /dev/null; then
            curl -fsSL https://raw.githubusercontent.com/tj/n/master/bin/n -o /usr/local/bin/n
            chmod +x /usr/local/bin/n
          fi
          sudo n ${NODE_VERSION}
          node -v
          npm -v
        '''
      }
    }

    stage('Install') {
      steps {
        sh 'npm ci'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test --if-present'
      }
      post {
        always {
          junit allowEmptyResults: true, testResults: '**/junit*.xml'
        }
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build --if-present'
      }
    }

    stage('Archive Artifact') {
      steps {
        script {
          def paths = sh(returnStdout: true, script: "ls -d dist build 2>/dev/null || true").trim()
          if (paths) {
            archiveArtifacts artifacts: 'dist/**, build/**', fingerprint: true, onlyIfSuccessful: true
          } else {
            echo 'No dist/build directories to archive.'
          }
        }
      }
    }
  }

  post {
    success { echo '✅ Build passed' }
    failure { echo '❌ Build failed' }
  }
}
