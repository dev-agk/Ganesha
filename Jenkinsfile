pipeline {
 agent any
 environment {
  dotnet = '/usr/bin/dotnet.exe'
 }
 stages {
  stage('Restore PACKAGES') {
   steps {
    sh "ls"
    sh "cd Ganesha"
    sh "dotnet restore"
   }
  }
  stage('Clean') {
   steps {
    sh 'dotnet clean'
   }
  }
  stage('Build') {
   steps {
    sh 'dotnet build --configuration Release'
   }
  }
 }
}