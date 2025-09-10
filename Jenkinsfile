pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Challakumar241/apachewebsite.git'
            }
        }
        stage('Ansible Install Apache') {
            steps {
                sh 'ansible-playbook -i hosts.ini apache-setup.yml'
            }
        }
        stage('Docker Build & Push') {
            steps {
                sh '''
                docker build -t apache-website:v1 .
                docker tag apache-website:v1 <your-dockerhub-username>/apache-website:v1
                docker push <your-dockerhub-username>/apache-website:v1
                '''
            }
        }
        stage('Deploy to K8s') {
            steps {
                sh 'kubectl apply -f deployment.yml'
            }
        }
    }
}
