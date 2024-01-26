pipeline {
    agent any

    stages {
        stage('Stash Files') {
            steps {
                // Stash files in the Jenkins workspace
                stash(name: 'myfiles', includes: 'week16-project/ansible-codes/*.yml')
            }
        }
        stage('Transfer Files to Ansible Agent') {
            agent {
                label 'ansible' // Execute this stage on the "ansible" agent
            }
            steps {
                // Unstash files on the "ansible" agent
                unstash 'myfiles'
                // Now you can use the transferred files on the agent
            }
        }
    }
}
