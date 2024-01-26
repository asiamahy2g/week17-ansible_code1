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
        
        stage('Zip and Push to JFrog Artifactory') {
            steps {
                script {
                    // Create a zip file containing all files except Jenkinsfile
                    sh 'zip -r ansible-codes.zip * -x Jenkinsfile'
                    
                    // Upload the zip file to JFrog Artifactory
                    sh 'curl -uadmin:AP4y6DmdHPZsNpu4PoxakusUxFz -T ansible-codes.zip "http://52.90.252.228:8081/artifactory/ansible-codes/"'
                }
            }
        }
    }
}
