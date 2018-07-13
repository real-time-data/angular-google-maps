apps = ['core'];
pipeline {
  agent none
  options {
    disableConcurrentBuilds()
    timeout(time: 60, unit: "MINUTES")
  }
  tools {
    nodejs '10.4.1'
  }
  environment {
    HOSTED_NPMRC = 'HOSTED_NPMRC'
  }
  stages {
    stage ('Build') {
      agent {
        label 'docker'
      }
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }
    stage('Publish') {
      agent {
        label 'docker'
      }
      steps {
          script {
            sshagent(['id_rsa_jenkins_gitlab']) {
                sh "cd dist/packages/core"
                configFileProvider ([configFile (fileId: HOSTED_NPMRC, targetLocation: '.npmrc')]) {
                    sh 'npm publish --verbose'
                }
            }
          }
      }
    }
  }
}
