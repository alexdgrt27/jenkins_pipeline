pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        MAVEN_HOME = "/opt/maven"
        IMAGE_NAME = "webapp:latest"
        CONTAINER_NAME = "webapp"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/alexdgrt27/jenkins_pipeline.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh """
                ${MAVEN_HOME}/bin/mvn clean package
                """
            }
        }

        stage('Docker Image Build') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME} .
                """
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                docker run -d -p 80:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                """
            }
        }
    }

    post {
        success {
            echo '✅ Webhook-triggered pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
        always {
            echo '📌 Pipeline execution finished.'
        }
    }
}
