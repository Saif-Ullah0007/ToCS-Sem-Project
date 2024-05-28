pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Application build stage...' 
            }
        }
        stage('Test') {
            steps {
                sh 'pwd'
                sh 'ls -la ${WORKSPACE}'
                // Remove existing files on the remote server first
                sh '''
                    gcloud compute ssh root@saif-project-apache-server --zone=us-central1-f -- "rm -rf /var/www/html/*"
                '''
                // Then copy new files from Jenkins workspace to the remote server
                sh '''
                    gcloud compute scp --recurse ${WORKSPACE}/* root@saif-project-apache-server:/var/www/html --zone=us-central1-f
                '''
            }
        }
        stage('Run') {
            steps {
                echo 'Application run stage' 
            }
        }
    }
}
