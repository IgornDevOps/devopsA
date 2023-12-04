pipeline {
    agent {
        label 'CentosSlave'
    }

    stages {

        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/IgornDevOps/devopsA.git']])
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t igorndevops/python-flask-docker:${env.BUILD_NUMBER} ."
            }
        }

        stage('Test Container') {
            steps {
                sh "docker run -itd -p 8080:8080 --name python-flask igorndevops/python-flask-docker:${env.BUILD_NUMBER} "
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
                sh "docker push igorndevops/python-flask-docker:${env.BUILD_NUMBER}"
            }
        }
    }
}    