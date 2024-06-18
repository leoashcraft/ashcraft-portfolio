pipeline {
	agent any

	environment {
		GIT_COMMIT_SHORT = "${sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()}"
	}

	stages {
		stage('Checkout') {
			steps {
				git branch: 'main', url: 'https://github.com/leoashcraft/ashcraft-portfolio'
			}
		}

		stage('Build and Deploy') {
			steps {
                script {
                    withCredentials([
                        string(credentialsId: 'laravel-app-key', variable: 'APP_KEY'),
                        usernamePassword(credentialsId: 'db-credentials', usernameVariable: 'DB_USERNAME', passwordVariable: 'DB_PASSWORD')
                    ]) {
                        sh "docker-compose down"

                        writeFile file: '.env.docker', text: """
                        APP_KEY=${APP_KEY}
                        DB_CONNECTION=mysql
                        DB_HOST=mysql
                        DB_PORT=3306
                        DB_DATABASE=ashcraft_portfolio
                        DB_USERNAME=${DB_USERNAME}
                        DB_PASSWORD=${DB_PASSWORD}
                        """

                        sh "docker-compose --env-file .env.docker up --build -d"
                    }
                }
			}
		}
	}
}