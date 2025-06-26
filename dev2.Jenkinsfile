pipeline {
    agent any 

    environment {
        IMAGE_NAME = "jenkins-test"
        ECR_REGISTRY = "886436945965.dkr.ecr.us-east-1.amazonaws.com"
        REGION = "us-east-1"
        TAG = "${new Date().format('yyyyMMddHHmmss)}"
    }

    options {
        timestamps()
    }

    stages{
        stage('Checkout') {
            steps {
                git url: 'https://github.com/vikyij/jenkins-demo', branch: 'main'
            }
        }

        stage('Login to ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-cred-id']])
                sh '''
                    echo "Logging into ecr..." 
                    aws ecr get-login-password --region $REGION | docker login --username AWS --password stdin $ECR_REGISTRY
                '''
            }
        }

        stage('Build and Push') {
            steps {
                sh '''
                    echo "Building Docker image..."
                    docker build -t $IMAGE_NAME:$TAG .

                    echo "Tagging Docker image..."
                    docket tag $IMAGE_NAME:TAG $ECR_REGISTRY/IMAGE_NAME:$TAG

                    echo "Pushing Docker Image to ECR..."
                    docker push $ECR_REGISTRY/$IMAGE_NAME:$TAG
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Image $IMAGE_NAME:$TAG pushed to $ECR_REGISTRY"
        }

        failure {
            echo "❌ Build failed. Check Docker, ECR, or credentials."
        }
    }
}