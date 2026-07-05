pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '992382545251'
        AWS_REGION = 'eu-central-1'
        IMAGE_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/flask-jenkins-demo:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/NetaVizenberg/flask-jenkins-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-jenkins-demo .'
            }
        }

        stage('Push Image to ECR') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'aws-credentials',
                    usernameVariable: 'AWS_ACCESS_KEY_ID',
                    passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                )]) {
                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                    docker tag flask-jenkins-demo:latest $IMAGE_URI
                    docker push $IMAGE_URI
                    '''
                }
            }
        }
    }
}

