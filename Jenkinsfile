pipeline {
 agent any
 environment {
  dotnet = '/usr/bin/dotnet.exe'
 }
 stages {
  stage('Restore PACKAGES') {
   steps {
    dir('Ganesha') {
      sh "ls"
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
      sh 'dotnet build --configuration Release'
      }
    }
  }
}
}