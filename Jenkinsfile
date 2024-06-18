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
                        string(credentialsId: 'APP_KEY', variable: 'APP_KEY'),
                        string(credentialsId: 'DB_DATABASE', variable: 'DB_DATABASE'),
                        string(credentialsId: 'DB_USERNAME', variable: 'DB_USERNAME'),
                        string(credentialsId: 'DB_PASSWORD', variable: 'DB_PASSWORD')
                    ]) {
                        sh "docker-compose down"

                        writeFile file: '.env.docker', text: """
                        APP_NAME=Laravel
                        APP_ENV=local
                        APP_KEY=${APP_KEY}
                        APP_DEBUG=true
                        APP_TIMEZONE=UTC
                        APP_URL=http://localhost

                        APP_LOCALE=en
                        APP_FALLBACK_LOCALE=en
                        APP_FAKER_LOCALE=en_US

                        APP_MAINTENANCE_DRIVER=file
                        APP_MAINTENANCE_STORE=database

                        BCRYPT_ROUNDS=12

                        LOG_CHANNEL=stack
                        LOG_STACK=single
                        LOG_DEPRECATIONS_CHANNEL=null
                        LOG_LEVEL=debug

                        DB_CONNECTION=mysql
                        DB_HOST=127.0.0.1
                        DB_PORT=3306
                        DB_DATABASE=${DB_DATABASE}
                        DB_USERNAME=${DB_USERNAME}
                        DB_PASSWORD=${DB_PASSWORD}

                        SESSION_DRIVER=database
                        SESSION_LIFETIME=120
                        SESSION_ENCRYPT=false
                        SESSION_PATH=/
                        SESSION_DOMAIN=null

                        BROADCAST_CONNECTION=log
                        FILESYSTEM_DISK=local
                        QUEUE_CONNECTION=database

                        CACHE_STORE=database
                        CACHE_PREFIX=

                        MEMCACHED_HOST=127.0.0.1

                        REDIS_CLIENT=phpredis
                        REDIS_HOST=127.0.0.1
                        REDIS_PASSWORD=null
                        REDIS_PORT=6379

                        MAIL_MAILER=log
                        MAIL_HOST=127.0.0.1
                        MAIL_PORT=2525
                        MAIL_USERNAME=null
                        MAIL_PASSWORD=null
                        MAIL_ENCRYPTION=null
                        MAIL_FROM_ADDRESS=leo@ashcraft.tech
                        MAIL_FROM_NAME="Leo Ashcraft"

                        AWS_ACCESS_KEY_ID=
                        AWS_SECRET_ACCESS_KEY=
                        AWS_DEFAULT_REGION=us-east-1
                        AWS_BUCKET=
                        AWS_USE_PATH_STYLE_ENDPOINT=false

                        VITE_APP_NAME="Leo Ashcraft"

                        """

                        sh "docker-compose --env-file .env.docker up --build -d"
                    }
                }
			}
		}
	}
}