pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

        stage('Validate Playbook') {
            steps {
                echo 'Validating Ansible Playbook...'
                sh 'ansible-playbook --syntax-check -i inventory.ini rhel-vm2.yml'
            }
        }
    }

    // The post block is outside of the stages block
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
