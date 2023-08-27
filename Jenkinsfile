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
                // SSH into the server and copy build files
                sshagent(credentials: ['your-ssh-credentials-id']) {
                    sh 'ssh -o StrictHostKeyChecking=no gazal94@10.0.1.70 "mkdir -p /home/gazal94/react"'
                    sh 'scp -o StrictHostKeyChecking=no -r build gazal94@10.0.1.70:/home/gazal94/react/'
                }
            }
        }

        stage('Ansible Playbooks') {
            steps {
                // Run Ansible playbooks
                sh 'ansible-playbook /home/gazal94/react/ansible-build-docker-lock.yaml'
                sh 'ansible-playbook /home/gazal94/react/ansible-build-docker-remote.yaml'
            }
        }
    }
}
