pipeline {
    agent any
    environment { 
        registry = "koppula18/petclinic" 
        registryCredential = 'dockerhub_id' 
        dockerImage = ''
    }
    stages {
        stage('Checkout Project'){
            steps {
                script {
                    checkout(
                        [
                            $class: 'GitSCM', 
                            branches: 
                            [
                                [name: "${params.BRANCH}"]
                            ], 
                            doGenerateSubmoduleConfigurations: false, 
                            extensions: [], 
                            submoduleCfg: [], 
                            userRemoteConfigs: 
                            [
                                [
                                    credentialsId: 'gitpass', 
                                    url: "https://github.com/KoppulaRajender/spring-petclinic.git"
                                ]
                            ]
                        ]
                    )
                }
            }
        }
        stage('Package Docker Image'){
            steps {
                script {
                    sh script: "cd spring-petclinic"
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


