pipeline {
  agent none
  parameters {
    booleanParam(name: "push", defaultValue: false)
  }
  options {
    timeout(time: 1, unit: 'HOURS')
    timestamps()
    ansiColor('xterm')
  }
  stages {
    agent {
        label 'linux && amd64 && docker'
    }
    stage('build') {
      steps {
        sh 'docker build -t docker/binfmt:${GIT_COMMIT} .'
      }
    }
    stage('push') {
      when {
        beforeAgent true
        expression { params.push }
      }
      steps {
        withDockerRegistry([credentialsId: 'docker-hub', url: "https://index.docker.io/v1/"]) {
          sh 'docker push docker/binfmt:${GIT_COMMIT}'
        }
      }
    }
  }
}
