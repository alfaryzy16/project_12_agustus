pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Ansible Syntax Check') {
            steps {
                sh '''
                    ansible-playbook \
                    -i ansible/inventory/production.ini \
                    ansible/playbooks/deploy.yml \
                    --syntax-check
                '''
            }
        }

        stage('Deploy Project 12') {
            steps {
                sshagent(credentials: ['vps-ssh-key']) {
                    sh '''
                        ansible-playbook \
                        -i ansible/inventory/production.ini \
                        ansible/playbooks/deploy.yml
                    '''
                }
            }
        }

    }

    post {
        success {
            echo 'PROJECT 12 DEPLOYMENT BERHASIL 🚀'
        }

        failure {
            echo 'PROJECT 12 DEPLOYMENT GAGAL ❌'
        }
    }
}
