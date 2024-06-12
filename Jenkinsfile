pipeline {
	agent any

	environment {
		GIT_COMMIT_SHORT = "${sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()}"
	}

	stages {
		stage('Checkout') {
			steps {
				git branch: 'main', url: 'https://github.com/leoashcraft/Jenkins-Laravel-Portfolio'
			}
		}

		stage('Build and Deploy') {
			steps {
				script {
					sh "docker-compose down"
					sh "docker-compose up --build -d"
				}
			}
		}
	}

	post {
		failure {
			// Future notifications
		}
	}
}