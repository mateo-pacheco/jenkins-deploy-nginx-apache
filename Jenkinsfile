
pipeline {
    agent any

    stages {
        stage('Create directory for the WEB Application') {
            steps {
                // First, drop the directory if exists
                sh 'rm -rf /home/jenkins/app-web'

                // Create the directory
                sh 'mkdir -p /home/jenkins/app-web'
            }
        }

        stage('Drop the containers') {
            steps {
                echo 'Dropping the containers...'
                sh 'docker rm -f app-web-apache || true'
                sh 'docker rm -f app-web-nginx || true'
            }
        }

        // Creating the containers in Parallel
        stage('Create the containers in Parallel') {
            parallel {
                stage('Create the Apache container') {
                    steps {
                        echo 'Creating the Apache Container...'
                        sh 'docker run -dit --name app-web-apache -p 9100:80 -v /home/jenkins/app-web:/usr/local/apache2/htdocs/ httpd'
                    }
                }

                stage('Create the Nginx container') {
                    steps {
                        echo 'Creating the Nginx Container...'
                        sh 'docker run -dit --name app-web-nginx -p 9200:80 -v /home/jenkins/app-web:/usr/share/nginx/html nginx'
                    }
                }
            }
        }

        // Copy the application
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'
                sh 'cp -r web/. /home/jenkins/app-web/'
            }
        }
    }
}
