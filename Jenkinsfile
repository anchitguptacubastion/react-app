pipeline {
    agent any

    environment {
        IMAGE_NAME = "react-app"
        REGISTRY = "harbor.cubastion.net"
        PROJECT = "test"

    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/your/frontend.git'
            }
        }

        stage('Install & Build') {
            steps {
                sh '''
                npm install
                npm run build
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $REGISTRY/$IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Docker Push') {
            steps {
                sh '''
                docker login -u $DOCKER_USER -p $DOCKER_PASS
                docker push $REGISTRY/$IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Deploy to Server') {
            steps {
                sh 'ssh ubuntu@yourserver "docker pull $REGISTRY/$IMAGE_NAME:$BUILD_NUMBER && docker compose -f frontend.yml up -d"'
            }
        }
    }
}
