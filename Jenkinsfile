pipeline {
  agent none
  options {
    disableConcurrentBuilds()
    timeout(time: 60, unit: "MINUTES")
  }
  tools {
    nodejs '10.4.1'
  }
  stages {
    stage ('Install') {
      agent {
        label 'docker'
      }
      steps {
        sh 'echo "hello world"'
      }
    }
  }
}
