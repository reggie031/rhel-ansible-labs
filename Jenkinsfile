pipeline {
    agent any

    stages {
        stage('Validate Playbook') {
            steps {
                echo 'Validating Ansible Playbook...'
                sh 'ansible-playbook --syntax-check -i inventory.ini rhel-vm2.yml'
            }
        }
    }
}
