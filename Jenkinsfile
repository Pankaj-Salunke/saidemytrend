pipeline {
         agent any
         
           environment {
              PATH = "/opt/maven/bin:$PATH"
           }
              
             stages{
              
              stage ('Build'){
                    steps {
                          sh 'mvn clean deploy'
                    }
              }

              stage ('SonarQube analysis'){
                    environment {
                          ScannerHome=tool'Saidemy-sonar-scanner'
                    }

              steps {
                     withSonarQubeEnv ('saidemy-sonarqube-server'){
                    sh "${scannerHome}/bin/sonar-scanner"
              } 
           }
        }
    }
 }

