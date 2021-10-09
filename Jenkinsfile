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
      sh 'dotnet publish -c release -r ganesha-ubuntu --self-contained true'
      sh 'dotnet publish -c release -r  ganesha-windows --self-contained true'    
      }
    }
  }
}
post {
        always {
            archiveArtifacts artifacts: 'Ganesha/Ganesha/bin/Release/netcoreapp3.1/ganesha-*/', onlyIfSuccessful: true
        }
    }
}