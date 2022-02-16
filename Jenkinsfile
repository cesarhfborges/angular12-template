pipeline {
  agent any
  options {
    timeout(time: 15, unit: 'MINUTES')
  }
  tools {nodejs "node14"}
  stages {

    stage ('prepare'){
      steps{
        checkout scm
      }
    }

    stage('Install modules') {
      steps { sh 'npm install' }
    }

    stage('Test') {
      parallel {
        stage('Static code analysis') {
            steps { sh 'npm run lint' }
        }
        stage('Unit tests') {
            steps { sh 'npm run test --watch=false --browsers=ChromeHeadless --no-progress' }
        }
      }
    }

    stage('Build') {
      steps { sh 'npm run build --prod' }
    }

    stage('Deploy') {
      steps {
        sh 'rm -rf ./node_modules'
        sh 'rm -rfv /var/www/darvsistemasraspberrypi/*'
        sh 'cp -R ./dist/ /var/www/darvsistemasraspberrypi/'
      }
    }
  }
}
