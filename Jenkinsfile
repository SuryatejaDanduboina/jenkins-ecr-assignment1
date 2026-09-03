pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '459338751693'
        ECR_REPO = 'jenkins-ecr-assignment'
        IMAGE_NAME = 'jenkins-ecr-image'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
            }
        }

        stage('Test') {
            steps {
                bat 'if exist index.html (echo Test Passed) else (echo Test Failed & exit /b 1)'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t jenkins-ecr-image:v1 .'
            }
        }

        stage('AWS Authentication') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials'
                ]]) {
                    bat 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 459338751693.dkr.ecr.us-east-1.amazonaws.com'
                }
            }
        }
    }
}
