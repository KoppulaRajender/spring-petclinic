pipeline {
    agent any
    environment { 
        registry = "koppula18/petclinic" 
        registryCredential = 'dockerhub_id' 
        dockerImage = ''
    }
    stages {
        stage('Cloning our Git') { 
            steps { 
                git branch: 'main', url: 'https://github.com/KoppulaRajender/spring-petclinic' 
            }
        }
        stage('Package Docker Image'){
            steps {
                script {
                    sh script: "cd spring-petclinic"
                    sh script: "git checkout main"
                    sh script: "./mvnw package"
                }
            }
        }
        
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }

        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
    }
    
}     


