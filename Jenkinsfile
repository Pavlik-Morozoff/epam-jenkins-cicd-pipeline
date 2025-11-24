pipeline {
    agent any

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
		sh "npm install"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing app'
		sh "npm test"
            }
        }
	stage('Build docker image') {
            steps {
                echo 'Building Docker images'
		sh "docker build -t ${IMAGE_NAME} ."
	    }
	}
	stage('Deploy') {
            steps {
                echo 'Deploying app'
		script {
                    sh "docker rm -f ${env.BRANCH_NAME} || true"
		    sh "docker run -d -p ${PORT}:${PORT} --name ${env.BRANCH_NAME} ${IMAGE_NAME}"
		}
	    }
	}
    }
}
