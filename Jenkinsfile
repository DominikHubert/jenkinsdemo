pipeline {
    agent any

    stages {
        
        
        stage('build') {
            when { expression { return currentBuild.currentResult == "SUCCESS" } }
            steps {
                script {
                echo "Build"
                app = docker.build "dkowatsch/app"
                //sh 'docker build . -t demosite'
                
                }
                
            }
        }
        stage('deploy and test') {
            when { expression { return currentBuild.currentResult == "SUCCESS" } }
            
                    steps {
                        script {
                        echo "deploy"
                    
                        docker.image('dkowatsch/app').withRun('-p 8081:80') {
                            sh 'curl localhost:8081'
                            }
                        }      
                    }
                
            
        }
        stage('publish') {
            when { expression { return currentBuild.currentResult == "SUCCESS" } }
            steps {
                script { 
                docker.withRegistry('https://registry.hub.docker.com', 'docker') {  
                app.push("latest")     
                }   
              }    
            }
        }
       
    }
}
