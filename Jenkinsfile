pipeline {
  agent any
  options {
    timeout(time: 10)
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
            steps { sh 'npm run test --watch=false --single-run' }
        }
      }
    }

    stage('Build') {
      steps { sh 'npm run build --prod' }
    }

    stage('Deploy') {
      steps { sh 'rm -rf ./node_modules' }
      steps { sh 'rm -rfv /var/www/darvsistemasraspberrypi/*' }
      steps { sh 'cp -R ./dist/ /var/www/darvsistemasraspberrypi/' }
    }
  }
}
