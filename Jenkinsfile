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
                configFileProvider ([configFile (fileId: HOSTED_NPMRC, targetLocation: 'dist/packages/core/.npmrc')]) {
                    dir ("dist/packages/core") {
                      sh "npm version 1.0.0-${BRANCH_NAME.replace("/", "-")}-${BUILD_NUMBER} --allow-same-version=true --git-tag-version=false --force"
                      sh 'npm publish --verbose'
                    }
                }
            }
          }
      }
    }
  }
}
