pipeline {
  agent any
  options {
    timeout(time: 1, unit: 'HOURS')
  }
  tools {nodejs "node14"}

  stages {

    stage('Install') {
      options {
        timeout(time: 5, unit: 'MINUTES')
      }
      steps { sh 'npm install' }
    }

    stage('Test') {
      options {
        timeout(time: 15, unit: 'MINUTES')
      }
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
      options {
        timeout(time: 5, unit: 'MINUTES')
      }
      steps { sh 'npm run build --prod' }
    }

    stage('Deploy') {
      options {
        timeout(time: 3, unit: 'MINUTES')
      }
      steps { sh 'rm -rfv /var/www/darvsistemasraspberrypi/*' }
      steps { sh 'cp -R /var/lib/jenkins/jobs/ /var/www/darvsistemasraspberrypi/' }
    }
  }
}
