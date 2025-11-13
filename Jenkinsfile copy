pipeline {
    agent any

    environment {
        IMAGE_NAME = "react-app"
        REGISTRY = "harbor.cubastion.net"
        PROJECT = "test"
        FULL_IMAGE = "${REGISTRY}/${PROJECT}/${IMAGE}"

    }

    stages {

        stage("Checkout Code") {
            steps {
                git "https://github.com/anchitguptacubastion/react-app"
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

        stage("Login to Harbor") {
            steps {
                sh """
                echo $HARBOR_PASS | docker login ${REGISTRY} -u $HARBOR_USER --password-stdin
                """
            }
        }

        stage("Push to Harbor") {
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
