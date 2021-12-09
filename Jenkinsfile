pipeline {
  agent {
    docker { image 'dlmurga/ds-11-build:1.0' }
  }
  tools { maven "m3" }
  stages {
    stage('Copy source code from git') {
      steps {
          git 'https://github.com/efsavage/hello-world-war'
      }
    }
  stage('Build war file') {
    steps {
      dir ('hello-world-war') {
        sh 'mvn package'
      }
    }
  }
  stage('Make docker image') {
    steps {
      dir ('hello-world-war') {
        git 'https://github.com/dlmurga/ds-11.git'
        sh 'cp ds-11/prod/Dockerfile .'
        sh 'docker build -t ds-11-prod .'
      }
      sh 'docker tag ds-11-prod:$version dlmurga/ds-11-prod:$version'
      sh 'docker login -u $docker_user -p $docker_pass'
      sh 'docker push dlmurga/ds-11-prod:$version'
    }
  }
  stage('Run docker on prod server') {
    steps {
      sh 'echo docker version'
    }
  }
  }
}