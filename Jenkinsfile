pipeline {
  agent any
  tools {nodejs "node14"}
  stages {

    stage('Install') {
      steps {
        timeout(time: 5, unit: 'MINUTES') {
          sh 'npm install'
        }
      }
    }

    stage('Test') {
      parallel {
        stage('Static code analysis') {
          steps {
            timeout(time: 5, unit: 'MINUTES') {
              sh 'npm run lint'
            }
          }
        }
        stage('Unit tests') {
          steps {
            timeout(time: 5, unit: 'MINUTES') {
              sh 'npm run test --watch=false'
            }
          }
        }
      }
    }

    stage('Build') {
      steps {
        timeout(time: 5, unit: 'MINUTES') {
          sh 'npm run build --prod'
        }
      }
    }

    stage('Deploy') {
      steps { sh 'rm -rfv /var/www/darvsistemasraspberrypi/*' }
      steps { sh 'cp -R /var/lib/jenkins/jobs/ /var/www/darvsistemasraspberrypi' }
    }
  }
}
