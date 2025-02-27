pipeline {
    agent any
    environment {
        ANSIBLE_PRIVATE_KEY = credentials('ansible_ssh_key') // Jenkins credential ID for SSH key
    }
    tools {
        git 'Git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/honey1417/ansible-docker-uc10.git'
            }
        }
      stage('Build Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_HUB_PSW', usernameVariable: 'DOCKER_HUB_USR')]) {
                    sh 'docker build -t $DOCKER_HUB_USR/demo-app:v5 .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_HUB_PSW', usernameVariable: 'DOCKER_HUB_USR')]) {
                    sh 'echo $DOCKER_HUB_PSW | docker login -u $DOCKER_HUB_USR --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_HUB_PSW', usernameVariable: 'DOCKER_HUB_USR')]) {
                    sh 'docker push $DOCKER_HUB_USR/demo-app:v5'
                }
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
