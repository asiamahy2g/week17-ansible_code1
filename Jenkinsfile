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
        //stage('Deploy to Nexus') {
           // steps {
             //   script {
               //     def nexusUrl = "http://18.234.103.149:8081/repository/code/"
                 //   def repositoryId = "code"
                   // def artifactPath = "code.zip"
                    
                   // withCredentials([usernamePassword(credentialsId: 'nexus-user-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                     //   def nexusUploadCommand = "curl -v -u \"$NEXUS_USERNAME:$NEXUS_PASSWORD\" -X POST \"$nexusUrl/service/rest/v1/components?repository=${repositoryId}\" -F \"raw.directory=\$(dirname '${artifactPath}')\" -F \"raw.asset1=@${artifactPath}\""
                       // sh nexusUploadCommand
                    //}
                //}
            }
        }    
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
        stage('Download Artifact from Jfrog and deploy on ansible server') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Download the zip file to JFrog Artifactory
                    sh 'curl -uadmin:AP4y6DmdHPZsNpu4PoxakusUxFz -O "http://3.86.200.5:8081/artifactory/ansible-codes/ansible-codes.zip/ec2-user/ansible-dev"'
                }
            }
        }

