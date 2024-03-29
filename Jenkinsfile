pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKER_REPO = "himanibisht/unisite"
        IMG = "himanibisht/devops-project-image"
        IMAGE_NAME = "himanibisht/devops-project-image"
        CONTAINER_NAME = "devops-project"
        GIT_REPO = "https://github.com/Himani1608/University-website.git"
        GIT_BRANCH = "main"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"

            }
        }
        stage('Build') {
            steps {
                script {
                    sh "sudo docker logout"
                    sh "sudo docker kill ${CONTAINER_NAME}|| true"
                    sh "sudo docker rm ${CONTAINER_NAME}|| true"
                    sh "sudo docker build . -t ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                sh "sudo docker run -it -p 100:80 --name ${CONTAINER_NAME} -d ${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }
        stage('Login') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }
        stage('Push') {
            steps {
                sh "sudo docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${DOCKER_REPO}:${BUILD_NUMBER}"
                sh "sudo docker push ${DOCKER_REPO}:${BUILD_NUMBER}"
                sh "sudo docker logout"
            }
        }
         stage('Clear Image'){
            steps {
                sh "sudo docker rmi ${IMG}:${BUILD_NUMBER.toInteger()-1}"
                sh "sudo docker rmi -f ${DOCKER_REPO}:${BUILD_NUMBER}"
            }
        }
    }
}
