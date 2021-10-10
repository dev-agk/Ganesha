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
      sh 'dotnet publish --configuration release' //-> run this if run time is installed
      // sh 'dotnet publish -c release -r ubuntu.20.04-x64 --self-contained true'
      // sh 'dotnet publish -c release -r  win-x64 --self-contained true'    
      }
    }
  }
  stage('deploy') {
    steps {
      dir('Ganesha') {
      sh 'sudo systemctl stop ganesha.service'
      sh 'sudo service nginx stop'
      sh 'sudo systemctl start ganesha.service'   
      sh 'sudo service nginx start'    
      }
    }
  }
}
post {
        always {
            archiveArtifacts artifacts: 'Ganesha/Ganesha/bin/Release/netcoreapp3.1/*', onlyIfSuccessful: true
        }
    }
}