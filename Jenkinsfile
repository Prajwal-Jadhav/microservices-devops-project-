pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t service-a:latest services/service-a/.'
                sh 'docker build -t service-b:latest services/service-b/.'
                sh 'docker build -t service-c:latest services/service-c/.'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run service-a pytest tests/'
                sh 'docker run service-b pytest tests/'
                sh 'docker run service-c pytest tests/'
            }
        }
        stage('Deploy') {
            steps {
                withKubeConfig([credentialsId: 'kube-config']) {
                    sh 'kubectl apply -f k8s/service-a-deployment.yaml'
                    sh 'kubectl apply -f k8s/service-b-deployment.yaml'
                    sh 'kubectl apply -f k8s/service-c-deployment.yaml'
                }
            }
        }
    }
}
