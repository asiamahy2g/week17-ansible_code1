pipeline {
    agent any

    stages {
        stage('Stash Files') {
            steps {
                 // Debugging output to list files in the workspace
                 sh 'ls -lR'
                // Stash files in the Jenkins workspace
                stash(name: 'myfiles', includes: '**/*', excludes: 'Jenkinsfile')
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
