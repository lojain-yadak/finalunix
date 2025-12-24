pipeline {
    agent any
    environment {
        
        LAMP_CONTAINER_NAME = 'apache_container'
        LOCAL_PROJECT_FOLDER = "${WORKSPACE}/unixfinalproject"
        PATH = "/usr/local/bin:${env.PATH}"  
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out repository...'
                git branch: 'main', url: 'https://github.com/lojain-yadak/unixfinalproject.git'
            }
        }

        stage('Build') {
            steps {
                echo 'No build step required for PHP.'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Starting Docker containers...'
                    sh 'docker compose up -d'  

                    echo 'Deploying files to container...'
                    sh "docker cp ${LOCAL_PROJECT_FOLDER}/. ${LAMP_CONTAINER_NAME}:/var/www/html"
                }
            }
        }

        stage('Restart Apache') {
            steps {
                echo 'Restarting Apache inside the container...'
                sh "docker exec ${LAMP_CONTAINER_NAME} service apache2 restart"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Deployment was successful.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
