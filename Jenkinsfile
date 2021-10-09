pipeline {
 agent any
  options {
    timeout(time: 15, unit: 'MINUTES') 
  }
 environment {
  dotnet = '/usr/bin/dotnet.exe'
 }
 stages {
  stage('Restore PACKAGES') {
   steps {
    dir('Ganesha') {
      sh "dotnet restore"
    }
   }
  }
  stage('Clean') {
   steps {
    dir('Ganesha') {
      sh 'dotnet clean'
    }
   }
  }
  stage('Build') {
    steps {
      dir('Ganesha') {
      sh 'dotnet publish -c release -r ubuntu.20.04-x64 --self-contained true'
      }
    }
  }
}
post {
        always {
            archiveArtifacts artifacts: '**/publish', onlyIfSuccessful: true
        }
    }
}