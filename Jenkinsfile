pipeline {
  agent any
  
  tools { nodejs "node" }
    
  stages {
    stage("Clone code from GitHub") {
      steps {
        script {
          checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/kaleyveeravenkatasatyanukarajkumar/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline.git']])
        }
      }
    }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('ECR Login') {
      steps {
        script {
          sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 148761646597.dkr.ecr.us-east-1.amazonaws.com'
        }
      }
    }

    stage('Build Node JS Docker Image') {
      steps {
        script {
          sh 'docker build -t kaleyveeravenkatasatyanukarajkumar/node-app-1.0 .'
        }
      }
    }

    stage('Push Docker Image to ECR') {
      steps {
        script {
          sh 'docker tag kaleyveeravenkatasatyanukarajkumar/node-app-1.0 148761646597.dkr.ecr.us-east-1.amazonaws.com/kaleyveeravenkatasatyanukarajkumar/node-app-1.0'
          sh 'docker push 148761646597.dkr.ecr.us-east-1.amazonaws.com/kaleyveeravenkatasatyanukarajkumar/node-app-1.0'
        }
      }   
    }

         
    stage('Deploying Node App to Minikube') {
      steps {
        script {
          // Start Minikube if it's not already running
          sh 'minikube start'
          
          // Set the context to Minikube
          sh 'kubectl config use-context minikube'
          
          // Check the namespaces
          sh "kubectl get ns"
          
          // Apply the Kubernetes configuration
          sh "kubectl apply -f nodejsapp.yaml"
        }
      }
    }
  }
}
