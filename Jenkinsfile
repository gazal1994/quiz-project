pipeline {
    agent any


   environment {
        GIT_HOME = tool name: 'Default', type: 'hudson.plugins.git.GitTool'
        PATH = "${GIT_HOME}/bin:${PATH}"
        GIT_CREDENTIALS = 'gazal94'  // Update with the correct credentials ID
        GIT_URL = 'https://github.com/gazal1994/quiz-project.git'
        SSH_PRIVATE_KEY = 'MIIEpAIBAAKCAQEAmg9dDJFv9qydDuiRFBxPUcneau7LWR/u5+GSv1TVPvzXZRzY/MQMQsyjwhgd/mKkI5ebzitvbgPIe2nEGTQOb/wN42ox8693tEkBqu1ovF2yHtDjxIWWRZCJjF/hX6I8iC6QAT2e3xHUPNAX4ODrUPRnqkAFo2/ptQPcjsijFAzwUfSbR+BlCaENTFMaJgq1NOdwSQjRLdh+bsjz+vrvApQOxu2bzMkpcMffgdq7Lo6anHRktCvQ9905M2R/KzS5dptnq+Gqu8pKIbKueNQWtG73ufUy+0kYJyvDc7kLwaqCkWp2+Ut0zEpwz+e76jVin9rFQrNdp7J8iWLO1B0tQQIDAQABAoIBAB1hvrpN5o+iHhYgzsKKH6qWmH/GaSvnRjYdNFKfSEhKCn5zQQL8FOaMhtLWrKm+gFV1bbalXAwcVpkK8+ZmojZDWRa9QyeZmHfe0J2bx7TdHcJ4zmfFnoJH7aPDCYWmuGq0jqxd5zXd/Z9XhKZT3y1CX579tNWV23m1cdQdedSx00cLAnARN7PossPoQTup89j1Ftfdo3liqHAtoboQRDjT01ilak7CKNeksnZ+B96Vyws1Yr16EfRoaUCq/3GS6yjLTkdHmb02/jOlZYzHVIg6L3IV4Dq8NI7J8ql8UIRhSnl8HA3fbFG2rYbPVBePSJGvNOATeY9BigcASexDsc0CgYEAx5H0zxMW+Xm7kaCVH3s/rYxAVVeyxgT9BbEyvu0PwczCiinbvjc8sdZhA8+1BLimWTLXhyyO+8d5T6KpYtdH9om4r/urn0fgvPnUU/9aYjTVbDjzRNBsArUKvoEBhqjwmRm3xURMiYTeK6efWOsWucIE/RuXvwrUAoKv7LjPK08CgYEAxZ8ezApwFX7jcQyJtQgAUAryB/pG1tlYSlUhA2SD4cECu1nvlIsINHfmUDEKm62vh2szlz1UYWBE1ARdd2Ci83kC7wSJV9pjLu1XG/DDT0mItzoINjWWSxEeGgkK1tMXs0xYz0hpr5e7Q03UHUzCCf26xoymD/vxWtzOramOum8CgYBwo+LnFcE74geKNHa2pBvW2nhdMviGZ75f/hnERY1FN0r+LI4ImKi7P2LWgd+L4KSTZ+zaML4rQfUoi4jLbvMBJc6GFahSaIFiaCf9mPzsvSFQyfwUdQbhqEl+KNYxqRzTbP6aauhAHiw/u4Zm65mSEv451d8aRwROvnCJTe4wZQKBgQCjQHaVgg3jhAXpbr0Xont3pAMa2gLJG5UXGsoB3ngf920FTh9fa5ckmOPW3RxxTILTcJiW2KArLPbO2qhHpLoYPaBRyUKYI52Jt6EdQMBncEyTaEo+VfhJHOPsCAo/OvS6NlirK5u65bJZwCJ47d7hmAxCVxAb5joUoJHP3mE82wKBgQCFsoklg717UtG+/Cc5kFPgdhkmYDKdxE27N7MQCjdcEgNNYgEJ7tszAau+b5NISOVquHC8VBDOtluVYIbDs+QMNL1Wwwv8XhgD4YBgs8RFTm8s0rsfh+5szI9/+82CsrMyZkyiQ4F/RRIZ9QD1b1Xe09421Wlby0RcktRUHIlNag=='

    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the repository using the provided credentials and Git installation
                    git  branch: 'main', credentialsId: env.GIT_CREDENTIALS, url: env.GIT_URL
                    sh 'git log'  // Print commit history
                    sh 'ls -lart' // List files in the workspace
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'sudo npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'sudo npm run build'
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(credentials: [env.SSH_PRIVATE_KEY]) {
                    sh 'ssh -o StrictHostKeyChecking=no gazal94@10.0.1.70 "mkdir -p /home/gazal94/react"'
                    sh 'scp -o StrictHostKeyChecking=no -r build gazal94@10.0.1.70:/home/gazal94/react/'
                }
            }
        }

        stage('Ansible Playbooks') {
            steps {
                sh 'ansible-playbook /home/gazal94/react/ansible-build-docker-lock.yaml'
                sh 'ansible-playbook /home/gazal94/react/ansible-build-docker-remote.yaml'
            }
        }
    }
}
