pipeline {
    agent none
    stages {
         stage('Clone Repo and Build the Flask App') {
            agent {
                label 'node-2'
            }
            steps {
                echo 'Clone and Build'
                script {
                    // Clone and Build the Flask App
                    ansiblePlaybook(
                        playbook: '/home/centos/jenkins-ansible-flask-1/03-centos-ubuntu.yml',
                        inventory: '/home/centos/jenkins-ansible-flask-1/hosts.ini'
                    )
                }
            }
        }
         stage('Deploy the Flask App with Ansible') {
            agent {
                label 'node-2'
            }
            steps {
                echo 'Start the Flask App'
                script {
                    // Start the Flask app
                    ansiblePlaybook(
                        playbook: '/home/centos/jenkins-ansible-flask-1/04-deploy-centos-ubuntu.yml',
                        inventory: '/home/centos/jenkins-ansible-flask-1/hosts.ini'
                    )
                }
            }
        }
    }
    post {
        success {
            script {
                // Send email for successful build
                mail to: 'kesienafels@gmail.com',
                     subject: "Build Successful - ${currentBuild.fullDisplayName}",
                     body: "Congratulations! The build was successful.\n\nCheck console output at ${BUILD_URL}"
            }
        }
        failure {
            script {
                // Send email for failed build
                mail to: 'kesienafels@gmail.com',
                     subject: "Build Failed - ${currentBuild.fullDisplayName}",
                     body: "Oops! The build failed.\n\nCheck console output at ${BUILD_URL}"
            }
        }
    }
}
