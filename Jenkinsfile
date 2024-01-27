pipeline {
    agent any

    stages {
        stage('Zip and Push to JFrog Artifactory') {
            steps {
                script {
                    // Create a zip file containing all files except Jenkinsfile
                    sh 'zip -r ansible-codes.zip * -x Jenkinsfile'
                    
                    // Upload the zip file to JFrog Artifactory
                    sh 'curl -uadmin:AP4y6DmdHPZsNpu4PoxakusUxFz -T ansible-codes.zip "http://3.86.200.5:8081/artifactory/ansible-codes"'
                }
            }
        }
        
        stage('Download Artifact from Jfrog') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Download the zip file from JFrog Artifactory
                    sh 'curl -uadmin:AP4y6DmdHPZsNpu4PoxakusUxFz -O "http://3.86.200.5:8081/artifactory/ansible-codes/ansible-codes.zip"'
                    // Now you can use the downloaded files on the agent
                }
            }
        }
        
        stage('Run playbook') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Unzip ansible-codes.zip
                    sh 'unzip -o ansible-codes.zip'
                    // Run ansible-playbook from the correct directory
                    dir('ansible-codes') {
                        sh 'ansible-playbook /home/ec2-user/ansible-dev/ansible-codes/first-playbook.yml'
                    }
                }
            }
        }
    }
}

