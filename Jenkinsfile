pipeline {
    agent any

    environment {
        // Change DEPLOY_IP to the actual IP address of your Deploy Server
        DEPLOY_SERVER = "ubuntu@13.62.101.115"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Use checkout scm to pull the current repository
                checkout scm
            }
        }

        stage('Copy Files') {
            steps {
                // Based on Step 3 (SSH Trust), this uses the Jenkins server's own SSH key.
                // We copy all 3 calculator files to /tmp/ on the deploy server
                sh '''
                scp -o StrictHostKeyChecking=no \
                index.html script.js style.css \
                ${DEPLOY_SERVER}:/tmp/
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ${DEPLOY_SERVER} "
                sudo mv /tmp/index.html /var/www/html/index.html
                sudo mv /tmp/script.js /var/www/html/script.js
                sudo mv /tmp/style.css /var/www/html/style.css

                sudo systemctl restart nginx
                "
                '''
            }
        }
    }
}
