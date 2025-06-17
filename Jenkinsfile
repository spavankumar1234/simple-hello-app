pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                echo 'Cloning Repo...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying Hello App...'
                sh 'mkdir -p /tmp/hello-deploy && cp index.html /tmp/hello-deploy/'
            }
        }
    }
}
