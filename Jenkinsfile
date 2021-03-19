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
                   docker.build registryNodejs + ":$BUILD_NUMBER" + " -f api/Dockerfile"
               }
            }
        }

	    stage('Build Angular App Container') {
	        steps {
		        script {
                    docker.build registryAngularApp + ":$BUILD_NUMBER" + " -f app-ui/Dockerfile"
		        }
	        }
	    }

        stage('Deploy to dockerhub') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        sh "docker push " + registryNodejs + ":$BUILD_NUMBER"
			            sh "docker push " + registryAngularApp + ":$BUILD_NUMBER"
                    }
                }
            }
        }
    
        stage('Remove all images and containers from agent') {
            steps {
                script {
                    sh "docker system prune -y"
                }
            }
        }

        stage('Get latest image from Dockerhub and Deploy') {
            steps {
                script {
		            sh "docker-compose up"
                }
            }
        }
    }
}
