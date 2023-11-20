pipeline {
    agent none
    stages {
        stage('Clone Repo and Build') {
            agent {
                label 'node-2'
            }
            steps {
                script {
                    // Clone and Build the App
                    sh 'ansible-playbook /home/centos/jenkins-ansible-flask-1/01-installations-flask.yml -i /home/centos/jenkins-ansible-flask-1/hosts.ini'
                }
            }
        }

        stage('Deploy with Ansible') {
            agent {
                label 'node-2'
            }
            steps {
                script {
                    // Start the app
                    sh 'ansible-playbook /home/centos/jenkins-ansible-flask-1/02-deploy-flask.yml -i /home/centos/jenkins-ansible-flask-1/hosts.ini'
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