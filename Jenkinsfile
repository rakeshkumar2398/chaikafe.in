pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ACCOUNT_ID = '561848282182'
        ECR_REPO = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/chai-kafe-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rakeshkumar2398/chaikafe.in.git'
            }
        }
        stage('Build Application') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build Image') {
            steps {
                sh 'docker build -t chaikafe.in-app:${IMAGE_TAG} .'
            }
        }

        stage('Amazon ECR Login') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Docker Tag') {
            steps {
                sh 'docker tag chaikafe.in-app:${IMAGE_TAG} $ECR_REPO:${IMAGE_TAG}'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push $ECR_REPO:${IMAGE_TAG}'
            }
        }
    }
}
