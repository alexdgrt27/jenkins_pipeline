pipeline {
    agent any

    environment {
        MAVEN_HOME = "/opt/maven"
        IMAGE_NAME = "webapp:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/USERNAME/html-css-docker-pipeline.git', branch: 'main'
            }
        }

        stage('Maven Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Deploy Container') {
            steps {
                sh "docker stop webapp || true"
                sh "docker rm webapp || true"
                sh "docker run -d -p 80:80 --name webapp ${IMAGE_NAME}"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished successfully!'
        }
    }
}
