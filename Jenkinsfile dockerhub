pipeline {
    agent any

    environment {
        DOCKER_USER = "anchitguptacubastion"
        IMAGE = "react-app"
        FULL_IMAGE = "docker.io/${DOCKER_USER}/${IMAGE}"

    }

    stages {

        stage("Checkout Code") {
            steps {
                git branch: 'main', url: 'https://github.com/anchitguptacubastion/react-app'
            }
        }

        stage("Build Docker Image") {
            steps {
                sh """
                docker build -t ${FULL_IMAGE}:${BUILD_NUMBER} .
                docker tag ${FULL_IMAGE}:${BUILD_NUMBER} ${FULL_IMAGE}:latest
                """
            }
        }

        stage("Login to Docker Hub") {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'DOCKER_HUB_CREDS',
            usernameVariable: 'DOCKERHUB_USER',
            passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh """
                    echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                    """
                }
            }
        }

        stage("Push to Docker Hub") {
            steps {
                sh """
                docker push ${FULL_IMAGE}:${BUILD_NUMBER}
                docker push ${FULL_IMAGE}:latest
                """
            }
        }

        // stage("Deploy on Server") {
        //     steps {
        //         sh """
        //         ssh sadmin@10.0.0.37 "
        //             docker login ${REGISTRY} -u ${HARBOR_USER} -p ${HARBOR_PASS} &&
        //             docker pull ${FULL_IMAGE}:latest &&
        //             docker compose -f frontend.yml up -d --force-recreate
        //         "
        //         """
        //     }
        // } 
    }
}
