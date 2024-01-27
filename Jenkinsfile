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
               
        stage('Unzip and Decrypt Files') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Check if ansible-codes directory exists
                    if (!fileExists('ansible-codes')) {
                        // Unzip ansible-codes.zip
                        sh 'unzip -o ansible-codes.zip'
                        
                        // Create flag file to indicate directory has been unzipped
                        writeFile file: 'ansible-codes.unzipped', text: ''
                    }
                    
                    // CD into ansible-codes folder
                    dir('ansible-codes') {
                        // Decrypt the inventory.yml file
                        sh 'ansible-vault decrypt /home/ec2-user/ansible-dev/workspace/week16-project/ansible-codes/inventory.yml --vault-password-file ../abc123.txt'
                    }
                }
            }
        }
        
        stage('Run playbook') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Run ansible-playbook from the correct directory
                    dir('ansible-codes') {
                        sh 'ansible-playbook /home/ec2-user/ansible-dev/workspace/week16-project/ansible-codes/first-playbook.yml'
                    }
                }
            }
        }
    }
}

def fileExists(String filename) {
    def file = new File(filename)
    return file.exists()
}

