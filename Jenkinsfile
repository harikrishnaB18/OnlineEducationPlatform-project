pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/harikrishnaB18/OnlineEducationPlatform-project.git'
            }
        }

        stage('Deploy index.html') {
            steps {
                sh '''
                echo "Deploying index.html to /var/www/html/"
                sudo cp index.html /var/www/html/
                '''
            }
        }
    }
}
