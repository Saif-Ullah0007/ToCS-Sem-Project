pipeline {
    agent any

    environment {
        GCLOUD_PROJECT = 'sixth-sequencer-423216-k8'
        GCLOUD_ZONE = 'us-central1-a'
        GCLOUD_INSTANCE = 'saif-project-apache-server'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Application build stage...'
            }
        }
        stage('Test') {
            steps {
                // Print the current working directory
                sh 'pwd'
                
                // List the contents of the Jenkins workspace
                sh 'ls -la ${WORKSPACE}'

                // Remove existing files on the remote server first
                sh """
                    gcloud compute ssh root@${GCLOUD_INSTANCE} --zone=${GCLOUD_ZONE} --project=${GCLOUD_PROJECT} -- "rm -rf /var/www/html/*"
                """

                // Then copy new files from Jenkins workspace to the remote server
                sh """
                    gcloud compute scp --recurse ${WORKSPACE}/* root@${GCLOUD_INSTANCE}:/var/www/html --zone=${GCLOUD_ZONE} --project=${GCLOUD_PROJECT}
                """
            }
        }
        stage('Run') {
            steps {
                echo 'Application run stage'
            }
        }
    }
}
