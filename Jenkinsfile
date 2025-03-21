def registry = 'https://pankaj7788.jfrog.io'

pipeline {                                    
    agent any                                 
    environment {                             
        PATH = "/opt/maven/bin:$PATH"         
    }                                         

    stages {                                  

        stage("build") {                      
            steps {                           
                echo "----------- build started ----------"  
                                              
                sh 'mvn clean deploy -Dmaven.test.skip=true'  
                                              
                echo "----------- build completed ----------"  
                                              
            }                                 
        }                                     

        stage("test") {                       
            steps {                           
                echo "----------- unit test started ----------"  
                                              
                sh 'mvn surefire-report:report'  
                                              
                echo "----------- unit test completed ----------"  
                                              
            }                                 
        }                                     

        stage('SonarQube analysis') {         
            environment {                     
                scannerHome = tool 'saidemy-sonar-scanner'  
                                              
            }                                 

            steps {                           
                withSonarQubeEnv('saidemy-sonarqube-server') {  
                                              
                    sh "${scannerHome}/bin/sonar-scanner"  
                                              
                }                             
            }                                 
        }                                   

        stage("Jar Publish") {                
            steps {                           
                script {                      
                    echo '<--------------- Jar Publish Started --------------->'  
                                              
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "artifact-cred"  
                                              
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"  
                                              
                    def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "pankaj-libs-release-local/{1}",
                              "flat": "false",
                              "props": "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""  
                                              // Defines the upload specification for uploading JAR files to Artifactory
                    def buildInfo = server.upload(uploadSpec)  
                                              // Uploads the files to Artifactory and collects build info
                    buildInfo.env.collect()  
                                              // Collects environment variables as part of the build info
                    server.publishBuildInfo(buildInfo)  
                                              // Publishes the build information to Artifactory
                    echo '<--------------- Jar Publish Ended --------------->'  
                                              // Logs a message indicating the end of JAR publishing
                }                             // 16  // Ends the script block for 'Jar Publish' stage
            }                                 // 15  // Ends the steps block for 'Jar Publish' stage
        }                                     // 14  // Ends the 'Jar Publish' stage

    }                                         // 3  // Ends the stages block
}                                             // 1  // Ends the pipeline block
