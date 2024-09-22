pipeline {
  agent any
  
  environment {
    AWS_REGION = 'us-west-2'
    AWS_ACCOUNT_ID = '975050024946'
    ECR_REGISTRY = "975050024946.dkr.ecr.us-west-2.amazonaws.com"
    FRONTEND_REPO = "975050024946.dkr.ecr.us-west-2.amazonaws.com/assignemnt/frontend-repo"
    BACKEND_REPO = "975050024946.dkr.ecr.us-west-2.amazonaws.com/assignment/backend-repo"
  }
  
  stages {
    stage('Build Docker Images') {
      steps {
        script {
          // Build the frontend image
          docker.build('frontend', './ResumeBuilderAngular')
          
          // Build the backend image
          docker.build('backend', './ResumeBuilderBackend')
        }
      }
    }
    
    stage('Push to ECR') {
      steps {
        script {
          // Push frontend image to ECR
          sh """
            docker tag frontend:latest ${FRONTEND_REPO}:latest
            docker push ${FRONTEND_REPO}:latest
          """
          
          // Push backend image to ECR
          sh """
            docker tag backend:latest ${BACKEND_REPO}:latest
            docker push ${BACKEND_REPO}:latest
          """
        }
      }
    }
    
    stage('Deploy to Kubernetes') {
      steps {
        script {
          // Apply Kubernetes configuration files
          sh 'kubectl apply -f k8s/'
        }
      }
    }
  }
}
