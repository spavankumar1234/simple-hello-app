pipeline {
    agent any

    environment {
        APP_NAME = "simple-hello-app"
        IMAGE_STREAM = "simple-hello-app-image"
        OPENSHIFT_SERVER = "https://api.cluster-6tcsp.6tcsp.sandbox1132.opentlc.com:6443"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/spavankumar1234/simple-hello-app.git'
            }
        }

        stage('Login to OpenShift') {
            steps {
                sh '''
                    echo "Logging into OpenShift..."
                    oc login --token=sha256~d2OS4Oc-hWEWkwx-a8mXQ-SGrFsR3SVY6TQI3Bqq__k \
                             --server=$OPENSHIFT_SERVER \
                             --insecure-skip-tls-verify=true
                '''
            }
        }

        stage('Build Image') {
            steps {
                sh '''
                    echo "Starting OpenShift Build..."
                    oc new-build --name=${IMAGE_STREAM} --binary --strategy=docker || echo "Build exists"
                    oc start-build ${IMAGE_STREAM} --from-dir=. --follow
                '''
            }
        }

        stage('Deploy App') {
            steps {
                sh '''
                    echo "Deploying to OpenShift..."
                    oc new-app ${IMAGE_STREAM} --name=${APP_NAME} || echo "App exists"
                    oc expose svc/${APP_NAME} || echo "Route exists"
                '''
            }
        }
    }
}

