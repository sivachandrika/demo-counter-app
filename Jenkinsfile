pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/sivachandrika/demo-counter-app.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('SonarQube analysis'){
            steps{   
                script{  
                    withSonarQubeEnv(credentialsId: 'sonarqube-auth') {   
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-auth'
                    }
                }
            }
            stage('Upload war file to nexus'){
                steps{
                    script{
                        nexusArtifactUploader artifacts: 
                        [
                            [
                                artifactId: 'springboot', 
                                classifier: '', file: 'target/Uber.jar', 
                                type: 'jar'
                                ]
                        ], 
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: '54.208.200.217:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'demoapp-release', 
                        version: '1.0.0'
                    }
                }
            }
        }
}
    