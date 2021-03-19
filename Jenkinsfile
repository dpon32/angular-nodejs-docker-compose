#!/bin/env groovy

pipeline {
    environment{
        registryNodejs = "dpontius32/nodejs"
	    registryAngularApp = "dpontius32/angular-app"
        registryCredential = 'dpontius32'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Build NodeJS Container') {
            steps {
		       script {
		           sh "cd api"
               }
               script {
                   dockerNodeImage = docker.build registryNodejs + ":$BUILD_NUMBER"
                }
                script {
                    sh "cd .."
                }
            }
        }

	    stage('Build Angular App Container') {
	        steps {
		        script {
		            sh "cd app-ui"
		        }
		        script {
		            dockerAngImage = docker.build registryAngularApp + ":$BUILD_NUMBER"
		        }
                script {
                    sh "cd .."
                }
	        }
	    }

        stage('Deploy to dockerhub') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerNodeImage.push()
			            dockerAngImage.push()
                    }
                }
            }
        }
    
        stage('Remove all images and containers from agent') {
            steps {
                script {
                    sh "docker system prune"
                }
            }
        }

        stage('Get latest image from Dockerhub and Deploy') {
            steps {
                script {
                    sh "cd .."
		            sh "docker-compose up"
                }
            }
        }
    }
}
