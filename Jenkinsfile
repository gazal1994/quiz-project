pipeline {
    agent any

environment {
        GIT_CREDENTIALS = credentials('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')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([$class: 'GitSCM', 
                              branches: [[name: '*/main']], 
                              userRemoteConfigs: [[url: 'https://github.com/gazal1994/quiz-project.git',
                                                  credentialsId: env.GIT_CREDENTIALS]]])
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
                sshagent(credentials: ['MIIEpAIBAAKCAQEAmg9dDJFv9qydDuiRFBxPUcneau7LWR/u5+GSv1TVPvzXZRzY
/MQMQsyjwhgd/mKkI5ebzitvbgPIe2nEGTQOb/wN42ox8693tEkBqu1ovF2yHtDj
xIWWRZCJjF/hX6I8iC6QAT2e3xHUPNAX4ODrUPRnqkAFo2/ptQPcjsijFAzwUfSb
R+BlCaENTFMaJgq1NOdwSQjRLdh+bsjz+vrvApQOxu2bzMkpcMffgdq7Lo6anHRk
tCvQ9905M2R/KzS5dptnq+Gqu8pKIbKueNQWtG73ufUy+0kYJyvDc7kLwaqCkWp2
+Ut0zEpwz+e76jVin9rFQrNdp7J8iWLO1B0tQQIDAQABAoIBAB1hvrpN5o+iHhYg
zsKKH6qWmH/GaSvnRjYdNFKfSEhKCn5zQQL8FOaMhtLWrKm+gFV1bbalXAwcVpkK
8+ZmojZDWRa9QyeZmHfe0J2bx7TdHcJ4zmfFnoJH7aPDCYWmuGq0jqxd5zXd/Z9X
hKZT3y1CX579tNWV23m1cdQdedSx00cLAnARN7PossPoQTup89j1Ftfdo3liqHAt
oboQRDjT01ilak7CKNeksnZ+B96Vyws1Yr16EfRoaUCq/3GS6yjLTkdHmb02/jOl
ZYzHVIg6L3IV4Dq8NI7J8ql8UIRhSnl8HA3fbFG2rYbPVBePSJGvNOATeY9BigcA
SexDsc0CgYEAx5H0zxMW+Xm7kaCVH3s/rYxAVVeyxgT9BbEyvu0PwczCiinbvjc8
sdZhA8+1BLimWTLXhyyO+8d5T6KpYtdH9om4r/urn0fgvPnUU/9aYjTVbDjzRNBs
ArUKvoEBhqjwmRm3xURMiYTeK6efWOsWucIE/RuXvwrUAoKv7LjPK08CgYEAxZ8e
zApwFX7jcQyJtQgAUAryB/pG1tlYSlUhA2SD4cECu1nvlIsINHfmUDEKm62vh2sz
lz1UYWBE1ARdd2Ci83kC7wSJV9pjLu1XG/DDT0mItzoINjWWSxEeGgkK1tMXs0xY
z0hpr5e7Q03UHUzCCf26xoymD/vxWtzOramOum8CgYBwo+LnFcE74geKNHa2pBvW
2nhdMviGZ75f/hnERY1FN0r+LI4ImKi7P2LWgd+L4KSTZ+zaML4rQfUoi4jLbvMB
Jc6GFahSaIFiaCf9mPzsvSFQyfwUdQbhqEl+KNYxqRzTbP6aauhAHiw/u4Zm65mS
Ev451d8aRwROvnCJTe4wZQKBgQCjQHaVgg3jhAXpbr0Xont3pAMa2gLJG5UXGsoB
3ngf920FTh9fa5ckmOPW3RxxTILTcJiW2KArLPbO2qhHpLoYPaBRyUKYI52Jt6Ed
QMBncEyTaEo+VfhJHOPsCAo/OvS6NlirK5u65bJZwCJ47d7hmAxCVxAb5joUoJHP
3mE82wKBgQCFsoklg717UtG+/Cc5kFPgdhkmYDKdxE27N7MQCjdcEgNNYgEJ7tsz
Aau+b5NISOVquHC8VBDOtluVYIbDs+QMNL1Wwwv8XhgD4YBgs8RFTm8s0rsfh+5s
zI9/+82CsrMyZkyiQ4F/RRIZ9QD1b1Xe09421Wlby0RcktRUHIlNag==
']) {
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
