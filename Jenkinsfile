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
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                      echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                      docker build -t apache-website:v1 .
                      docker tag apache-website:v1 $DOCKER_USER/apache-website:v1
                      docker push $DOCKER_USER/apache-website:v1
                    """
                }
            }
        }
        stage('Deploy to K8s') {
            steps {
                sh 'kubectl --kubeconfig=/var/lib/jenkins/.kube/config apply -f deployment.yml'
            }
        }
    }
}


