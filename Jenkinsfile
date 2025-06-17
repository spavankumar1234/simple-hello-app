pipeline {
    agent any

    environment {
        APP_NAME = "simple-hello-app"
        IMAGE_STREAM = "simple-hello-app-image"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/spavankumar1234/simple-hello-app.git'
            }
        }

        stage('Login to OpenShift') {
            steps {
                sh 'oc login --token=sha256~d2OS4Oc-hWEWkwx-a8mXQ-SGrFsR3SVY6TQI3Bqq__k --server=$OPENSHIFT_SERVER --insecure-skip-tls-verify=true'
            }
        }

        stage('Build Image') {
            steps {
                sh '''
                oc new-build --name=${IMAGE_STREAM} --binary --strategy=docker || echo "Build exists"
                oc start-build ${IMAGE_STREAM} --from-dir=. --follow
                '''
            }
        }

        stage('Deploy App') {
            steps {
                sh '''
                oc new-app ${IMAGE_STREAM} --name=${APP_NAME} || echo "App exists"
                oc expose svc/${APP_NAME} || echo "Route exists"
                '''
            }
        }
    }
}
