pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/sumeetp121-devops/flask-devops-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-devops-app .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker tag flask-devops-app $DOCKER_USER/flask-devops-app:latest'
                    sh 'docker push $DOCKER_USER/flask-devops-app:latest'
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook deploy.yml -i inventory'
            }
        }
    }
}

