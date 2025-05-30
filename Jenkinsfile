pipeline {
    agent any

    environment {
        REMOTE_USER = 'ubuntu'
        REMOTE_IP = '15.207.183.246' // <-- Your EC2 IP
        REMOTE_PATH = '/var/www/html/'
        PRIVATE_KEY = '~/.ssh/id_rsa'
        S3_BUCKET = 'online-education-platform-1' // <-- Your bucket name
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
                echo "Copying index.html to EC2 instance"
                scp -o StrictHostKeyChecking=no -i $PRIVATE_KEY index.html $REMOTE_USER@$REMOTE_IP:$REMOTE_PATH
                '''
            }
        }

        stage('Upload to S3') {
            steps {
                sh '''
                echo "Uploading index.html to S3"
                aws s3 cp index.html s3://$S3_BUCKET/index.html --acl public-read
                '''
            }
        }
    }
}
