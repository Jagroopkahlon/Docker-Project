pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'petclinic:v1.1'
        DOCKER_TAG = 'jagroopsingh/jenkins-project:v1.1'
        JAR_PATH = '**/**.jar'
    }

    stages {
        stage('Build and Deploy with Maven') {
            steps {
                script {
                    sh 'mvn deploy'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} -f Dockerfile-multistage .'
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh 'docker tag ${DOCKER_IMAGE} ${DOCKER_TAG}'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push ${DOCKER_TAG}'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -d -p 1234:8080 --name petclinic ${DOCKER_IMAGE}'
                    sh 'sleep 10'
                }
            }
        }

        stage('Check Running Containers') {
            steps {
                script {
                    sh 'docker ps'
                }
            }
        }

        stage('Stop and Remove Docker Container') {
            steps {
                script {
                    sh 'docker stop petclinic'
                    sh 'docker rm petclinic'
                }
            }
        }

        stage('Remove Docker Images') {
            steps {
                script {
                    sh 'docker rmi ${DOCKER_IMAGE}'
                    sh 'docker rmi ${DOCKER_TAG}'
                }
            }
        }

        stage('List Docker Images') {
            steps {
                script {
                    sh 'docker images'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: "${JAR_PATH}", allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
