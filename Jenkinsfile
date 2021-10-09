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
      sh 'dotnet publish -c Release'
      }
    }
  }
}
}