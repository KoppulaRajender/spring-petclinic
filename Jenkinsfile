pipeline {
    agent any
    environment { 
        registry = "koppula18/petclinic" 
        registryCredential = 'dockerhub_id' 
        dockerImage = ''
    }
    options {
        ansiColor('xterm')
        timestamps()
        skipDefaultCheckout()
        skipStagesAfterUnstable()
        buildDiscarder(logRotator(numToKeepStr: '14', daysToKeepStr: '14'))
    }
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Choose branch name')
    }
    stages {
        stage('Ensure clean workspace'){
            steps {
                script {
                    getBuildUser()
                    currentBuild.displayName = "#${currentBuild.number} | ${env.BUILD_USER} | ${params.BRANCH}" 
                    deleteDir()
                }
            }
        }
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


