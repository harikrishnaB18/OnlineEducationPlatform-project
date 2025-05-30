pipeline {
    agent any

    environment {
        REMOTE_USER = 'ubuntu'
        REMOTE_IP = '15.207.183.246'           // Your EC2 IP
        REMOTE_TEMP_PATH = '/home/ubuntu/'     // Temporary upload path
        REMOTE_DEPLOY_PATH = '/var/www/html/'  // Final deploy path
        PRIVATE_KEY = '~/.ssh/id_rsa'           // Jenkins private key path
        S3_BUCKET = 'online-education-platform-1'  // Your S3 bucket name
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/harikrishnaB18/OnlineEducationPlatform-project.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                echo "Copying index.html to EC2 instance temporary folder"
                scp -o StrictHostKeyChecking=no -i $PRIVATE_KEY index.html $REMOTE_USER@$REMOTE_IP:$REMOTE_TEMP_PATH

                echo "Moving index.html to /var/www/html/ with sudo"
                ssh -o StrictHostKeyChecking=no -i $PRIVATE_KEY $REMOTE_USER@$REMOTE_IP "sudo mv ${REMOTE_TEMP_PATH}index.html ${REMOTE_DEPLOY_PATH} && sudo chown www-data:www-data ${REMOTE_DEPLOY_PATH}index.html"
                '''
            }
        }

        stage('Upload to S3') {
            steps {
                sh '''
                echo "Uploading index.html to S3 without ACL"
                aws s3 cp index.html s3://$S3_BUCKET/index.html
                '''
            }
        }
    }
}
