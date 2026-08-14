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

                    withCredentials([
                        file(
                            credentialsId: 'ansible-vault-file',
                            variable: 'VAULT_FILE'
                        ),
                        string(
                            credentialsId: 'ansible-vault-password',
                            variable: 'VAULT_PASSWORD'
                        )
                    ]) {

                        sh '''
                            cp "$VAULT_FILE" ansible/group_vars/vault.yml

                            printf '%s' "$VAULT_PASSWORD" > .vault-password

                            chmod 600 ansible/group_vars/vault.yml
                            chmod 600 .vault-password

                            ansible-playbook \
                            -i ansible/inventory/production.ini \
                            ansible/playbooks/deploy.yml \
                            --vault-password-file .vault-password

                            rm -f .vault-password
                            rm -f ansible/group_vars/vault.yml
                        }
                    }
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