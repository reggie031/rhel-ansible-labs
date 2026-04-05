pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from VM1...'
                git url: 'git@VM1_IP:/srv/git/ops-scripts.git', branch: 'main', credentialsId: 'your-credentials-id'
            }
        }

        stage('Validate Playbook') {
            steps {
                echo 'Validating Ansible Playbook...'
                sh 'ansible-playbook --syntax-check -i inventory.ini rhel-vm2.yml'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
