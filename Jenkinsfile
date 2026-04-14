pipeline {
    agent any

    environment {
        IMAGE_NAME = "vishnukbhasker/space-3d-site"
        TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vishnukbhasker/space-3d-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {E:
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:$TAG'
                }
            }
        }

        stage('Deploy to Minikube') {
    steps {
        sh '''
            export KUBECONFIG=/var/lib/jenkins/.kube/config
            kubectl get nodes
            kubectl apply -f k8s/deployment.yaml
            kubectl apply -f k8s/service.yaml
            kubectl rollout status deployment/space-3d-site
        '''
    }
}
    }
}
