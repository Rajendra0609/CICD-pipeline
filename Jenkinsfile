pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "daggu1997/train"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
            }
        }
        stage('test') {
            steps {
                echo 'Running test automation'
                sh './gradlew npm_test'
          }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    sh 'docker push daggu1997/train:latest'
                    }
                }
            }
        }
        stage('Clean') {
            steps {
                deleteDir() // Clean the workspace
                sh 'echo "Clean the repository"' // Display message about cleaning the repository
            }
        }
    }
}
