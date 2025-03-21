pipeline {
         agent any
         
           environment {
              PATH = "/opt/maven/bin:$PATH"
           }
              
             stages{
              
              stage ("Build"){
                    steps {
                          echo "-------Build started-----"
                          sh 'mvn clean deploy -Dmaven.test.skip=true'
                          echo "-------Build completed-----"
                    }
              }

              stage ("test"){
                    steps {
                          echo "-------unit test started-----"
                          sh 'mvn surefire-report:report'
                          echo "-------unit test completed-----"
                    }
              }

              stage ("SonarQube analysis"){
                    environment {
                          ScannerHome=tool'Saidemy-sonar-scanner'
                    }

              steps {
                     withSonarQubeEnv ('Saidemy-sonarQube-server'){
                    sh "${scannerHome}/bin/sonar-scanner"
              } 
           }
      }
               stage ("Quality Gate"){
                    steps {
                         script{
                           timeout (time: 1, unit: 'HOURS') {
                            def qg= waitForQualityGate()
                          if (qg.status!='ok') {
                        error "pipeline aborted due to quality gate failure:${qg.status}"
                }
              }
            }
          }
        }
     }
  }
