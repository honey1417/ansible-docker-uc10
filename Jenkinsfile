pipeline {
    agent any

    environment {
        DOCKER_HUB_USR = credentials('docker-creds')  // Jenkins credential ID for Docker username
        DOCKER_HUB_PSW = credentials('docker-creds')  // Jenkins credential ID for Docker password
        ANSIBLE_PRIVATE_KEY = credentials('ansible_ssh_key') // Jenkins credential ID for SSH key
    }
    tools {
        git 'Git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/honey1417/ansible-docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/demo-app:v4 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKER_HUB_PASS | docker login -u $DOCKER_HUB_USER --password-stdin'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push $DOCKER_HUB_USER/demo-app:v4'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook (
                    playbook: 'playbook2.yml',
                    extras: '--private-key=$ANSIBLE_PRIVATE_KEY -u jenkins'
                )                
            }
        }
    }
}
