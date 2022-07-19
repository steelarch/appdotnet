pipeline {
    agent any
    environment {
        IMAGENAME='steelarch/appdot'
        MAJOR='1'
        MINOR='0'
    }
    stages {
        stage('checkout code repo') {
            steps {
                checkout scm
            }
        }
        stage('Build Container') {
            steps {
                script{
                 app = docker.build(IMAGENAME)
                }
            }
        }
        stage('Docker push to DockerHub') {
            steps {
                script{
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub') {
                    app.push("$MAJOR.$MINOR.${GIT_COMMIT.take(8)}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}