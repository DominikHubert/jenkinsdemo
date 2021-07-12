pipeline {
    agent any

    stages {
         stage('linting') {


            steps {
                // Get some code from a GitHub repository
                 // git 'https://github.com/DominikHubert/jenkins.git'
                sh 'ls'
                sh 'csslint --format=checkstyle-xml mystyle.css > results.xml'
            }
            post {
                always{
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                recordIssues(tools: [cssLint(pattern: 'results.xml')], qualityGates: [[threshold: 3, type: 'TOTAL', unstable: true]], healthy: 10, unhealthy: 100, minimumSeverity: 'HIGH')
                archiveArtifacts 'results.xml'
                }
            }
        
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
