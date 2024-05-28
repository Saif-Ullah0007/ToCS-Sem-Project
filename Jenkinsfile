pipeline {
    agent any
    environment {
        REMOTE_SERVER = 'root@saif-project-apache-server'
        REMOTE_ZONE = 'us-central1-f'
        REMOTE_PATH = '/var/www/html'
    }
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
                    gcloud compute ssh ${REMOTE_SERVER} --zone=${REMOTE_ZONE} -- "rm -rf ${REMOTE_PATH}/*"
                '''
                // Then copy new files from Jenkins workspace to the remote server
                sh '''
                    gcloud compute scp --recurse ${WORKSPACE}/* ${REMOTE_SERVER}:${REMOTE_PATH} --zone=${REMOTE_ZONE}
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
