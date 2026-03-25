pipeline {
    agent any

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_USERNAME = "sabirsaheel0"
        DOCKERHUB_REPO_NAME = "frontend-capstone"
    }

    stages {

        stage('BUILD DOCKER IMAGE') {
            steps {
                echo '---building docker image---'
                sh 'docker build -t ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('DOCKER LOGIN + PUSH') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    passwordVariable: 'pass',
                    usernameVariable: 'uname'
                )]) {
                    sh 'echo ${pass} | docker login -u ${uname} --password-stdin'
                }

                sh 'docker push ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO_NAME}:${IMAGE_TAG}'
                sh 'docker logout'
                echo 'pushing docker image'
            }
        }

        stage('clean docker images') {
            steps {
                sh 'docker rmi ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO_NAME}:${IMAGE_TAG}'
            }
        }

    }
}