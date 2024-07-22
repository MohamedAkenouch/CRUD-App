pipeline {
    agent any

    environment {
        SPRING_DATASOURCE_URL = credentials('SPRING_DATASOURCE_URL')
        SPRING_DATASOURCE_USERNAME = credentials('SPRING_DATASOURCE_USERNAME')
        SPRING_DATASOURCE_PASSWORD = credentials('SPRING_DATASOURCE_PASSWORD')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    def envVars = [
                        "SPRING_DATASOURCE_URL=${SPRING_DATASOURCE_URL}",
                        "SPRING_DATASOURCE_USERNAME=${SPRING_DATASOURCE_USERNAME}",
                        "SPRING_DATASOURCE_PASSWORD=${SPRING_DATASOURCE_PASSWORD}"
                    ]

                    // Write env vars to a file
                    writeFile file: '.env', text: envVars.join("\n")

                    // Run docker-compose
                    sh 'docker-compose up -d --build'
                }
            }
        }
    }

    post {
        always {
            sh 'docker-compose down'
        }
    }
}
