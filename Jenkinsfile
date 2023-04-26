pipeline {
    agent any
    environment {
//         PORT = "85"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKERHUB_IMAGE= "himanibisht/devops-project"
        IMAGE_NAME = "devops-project-image"
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
                    sh "sudo docker kill ${CONTAINER_NAME}|| true"
                    sh "sudo docker rm ${CONTAINER_NAME}|| true"
                    sh "sudo docker build . -t ${IMAGE_NAME}"
                    sh "sudo docker build . -t ${DOCKERHUB_IMAGE}"
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                sh "sudo docker run -it -p 287:80 --name ${CONTAINER_NAME} -d ${IMAGE_NAME}"
            }
        }
        stage('Login and push') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "sudo docker push ${IMAGE_NAME}"
            }
        }
    }
}
