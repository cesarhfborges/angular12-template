pipeline {
  agent any
  tools {nodejs "node14"}
  stages {

    stage('Install') {
      steps { sh 'npm install' }
    }

    stage('Test') {
      parallel {
        stage('Static code analysis') {
            steps { sh 'npm run lint' }
        }
        stage('Unit tests') {
            steps { sh 'npm run test --watch=false' }
        }
      }
    }

    stage('Build') {
      steps { sh 'npm run build --prod' }
    }

    stage('Deploy') {
      steps { sh 'rm -rfv /var/www/darvsistemasraspberrypi/*' }
      steps { sh 'cp -R /var/lib/jenkins/jobs/ /var/www/darvsistemasraspberrypi' }
    }
  }
}
