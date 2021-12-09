pipeline {
  agent {
    docker {
      image 'dlmurga/ds-11-build:1.0'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  tools { maven "m3" }
  stages {
    stage('Copy source code from git') {
      steps {
          git 'https://github.com/efsavage/hello-world-war'
          sh 'ls -l'
      }
    }
  stage('Build war file') {
    steps {
        sh 'mvn package'
    }
  }
  stage('Make docker image') {
    steps {
      sh 'mkdir -p df_folder'
      dir ('df_folder') {
        git 'https://github.com/dlmurga/ds-11.git'
        sh 'ls -l'
      }
      sh 'docker build -t ds-11-prod -f df_folder/prod/Dockerfile .'
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