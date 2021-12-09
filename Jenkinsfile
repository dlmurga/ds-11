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
      sh 'mkdir -p dc_folder'
      dir ('df_folder') {
        git 'https://github.com/dlmurga/ds-11.git'
        sh 'ls -l'
      }
      sh 'docker --config dc_folder/  build -t ds-11-prod:$version -f df_folder/prod/Dockerfile .'
      sh 'docker --config dc_folder/ tag ds-11-prod:$version dlmurga/ds-11-prod:$version'
      sh 'docker --config dc_folder/ login -u $docker_user -p $docker_pass'
      sh 'docker --config dc_folder/ push dlmurga/ds-11-prod:$version'
    }
  }
  stage('Run docker on prod server') {
    steps {
      script {
  	    def remote = [:]
  	    remote.name = 'prod'
  	    remote.host = '$ip_address'
  	    remote.allowAnyHosts = true
  	    withCredentials([usernamePassword(credentialsId: 'prod_root', passwordVariable: 'password', usernameVariable: 'username')]) {
   		  remote.user = "${username}"
   		  remote.password = "${password}"
  	    }
  	    sshCommand remote: remote, command: "pwd"
  	  }
  	}
  }
}