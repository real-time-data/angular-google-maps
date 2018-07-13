apps = ['core', 'js-marker-clusterer', 'snazzy-info-window'];
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
            def branches = [:]
            apps.each { app ->
                branches["${app}"] = {
                sshagent(['id_rsa_jenkins_gitlab']) {
                    configFileProvider ([configFile (fileId: HOSTED_NPMRC, targetLocation: '.npmrc')]) {
                        sh "cd dist/packages/${app}"
                        sh 'npm publish --verbose'
                    }
                }
                }
            }
            parallel branches
        }
      }
    }
  }
}
