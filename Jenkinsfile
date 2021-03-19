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
		    cd api
		}
		script {
		    mv Dockerfile-dev Dockerfile
		}
                script {
                    dockerNodeImage = docker.build Dockerregistry + ":$BUILD_NUMBER"
                }
            }
        }

	stage('Build Angular App Container') {
	    steps {
		script {
		    sh "cd ap-ui"
		}
		script {
		    sh "mv Dockerfile-dev Dockerfile"
		}
		script {
		    dockerAngImage = docker.build Dockerregistry + ":$BUILD_NUMBER"
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
