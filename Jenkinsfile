pipeline {
    agent {
        label 'CentosSlave'
    }

    stages {

        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: 'master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/IgornDevOps/devopsA.git']])
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t python-flask-docker:${env.BUILD_NUMBER} ."
            }
        }

        stage('Test Container') {
            steps {
                sh "docker run -itd -p 8080:8080 --name python-flask python-flask-docker:${env.BUILD_NUMBER} "
                sleep 5
                sh "curl localhost:8080"
                sh "docker stop python-flask && docker rm python-flask"
            }
        }
