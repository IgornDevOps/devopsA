pipeline {
    agent {
        label 'CentosSlave'
    }

    stages {

        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: 'master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/lidorg-dev/python-flask-docker.git']])
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t lidorlg/python-flask-docker:${env.BUILD_NUMBER} ."
            }
        }

        stage('Test Container') {
            steps {
                sh "docker run -itd -p 8080:8080 --name python-flask lidorlg/python-flask-docker:${env.BUILD_NUMBER} "
                sleep 5
                sh "curl localhost:8080"
                sh "docker stop python-flask && docker rm python-flask"
            }
        }

        stage('Publish Image') {
            environment {
                DOCKERHUB_CREDS = credentials('docker-hub-creds')
            }
            steps {
                sh "docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW"
                sh "docker push lidorlg/python-flask-docker:${env.BUILD_NUMBER}"
            }
        }
    }
}