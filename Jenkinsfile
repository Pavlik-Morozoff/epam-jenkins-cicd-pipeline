pipeline {

    agent any

    tools {
        nodejs 'node'
    }

    environment {
        IMAGE_NAME = "node${env.BRANCH_NAME}:v1.0"
	PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"

    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Scanning branches'
		checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building app'
		bat "npm install"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing app'
		bat "npm test"
            }
        }
	stage('Build docker image') {
            steps {
                echo 'Building Docker images'
		bat "docker build -t ${IMAGE_NAME} ."
	    }
	}
	stage('Deploy') {
            steps {
                echo 'Deploying app'
		script {
                    bat "docker rm -f ${env.BRANCH_NAME} || true"
		    bat "docker run -d -p ${PORT}:${PORT} --name ${env.BRANCH_NAME} ${IMAGE_NAME}"
		}
	    }
	}
    }
}
