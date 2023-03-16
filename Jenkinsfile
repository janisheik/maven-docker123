pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/janisheik/maven-docker123.git']]])
                  sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t jani180348/kubernetes .'
                }
            }
        }
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'jani180348', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u jani180348 -p ${dockerhubpwd}'
                        
                    }
                    sh 'docker push jani180348/kubernetes'
                }
            }
        }
        stage('run image') {
            steps{          
                sh 'docker run  -d -t --name kubernetes -p 8081:80 jani180348/kubernetes:latest'  
                }
       }
    }    
}
