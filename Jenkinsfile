pipeline{
    
    agent any 
    
    stages{
        
        stage('git checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/SHUBHAM-KALEKAR/demo-counter-app-01.git'
                }
            }
        }
        stage('Unit Test'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        
        stage('Integration Testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
            
        }
        
        stage('build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        
        stage('sonarqube analysis'){
            
            steps{
                
                script{
                    
                    
                    withSonarQubeEnv(credentialsId: 'sonar') {
                        
                    sh 'mvn clean package sonar:sonar'
    
                    }
                    
                    
                }
                
            
            }
        }
        
        stage('Quality Gate status'){
            
            steps{
                
                script{
                    
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }
            }
        }

        
            
    

        stage('dcoker image Build'){

            steps{

                script{

                    sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID shubhamkalekar51/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID shubhamkalekar51/$JOB_NAME:v1.latest'

                }
            }
        }

        stage('push image to dockerhub'){

            steps{

                script{

                    withCredentials([string(credentialsId: 'f5ea4ef1-5aa6-40ff-8717-8d20943518fa', variable: 'docker_cred')]) {

                        sh "docker login -u shubhamkalekar51 -p ${docker_cred}"
                          sh "docker image push shubhamkalekar51/$JOB_NAME:v1.$BUILD_ID"
                          sh "docker image push shubhamkalekar51/$JOB_NAME:v1.latest"
             
                  }
                }
            }
        }
    }
}
