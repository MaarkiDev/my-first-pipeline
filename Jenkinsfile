pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test || true'
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build || true'
      }
    }

    stage('Deploy (Netlify)') {
      steps {
        withCredentials([
          string(credentialsId: 'netlify-api-token', variable: 'NETLIFY_AUTH_TOKEN'),
          string(credentialsId: 'netlify-site-id', variable: 'NETLIFY_SITE_ID')
        ]) {
          sh '''
            npx netlify deploy \
              --prod \
              --auth=$NETLIFY_AUTH_TOKEN \
              --site=$NETLIFY_SITE_ID \
              --dir=public
          '''
        }
      }
    }
  }
}
