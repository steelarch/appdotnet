pipeline {
    agent any
    environment {
        IMAGENAME='steelarch/appdot'
        MAJOR='1'
        MINOR='0'
        scannerHome = tool 'dotnet-sonarscanner'
    }
    stages {
        stage('checkout code repo') {
            steps {
                checkout scm
            }
        }
        stage('Sonar Scanner') {   
            steps {
                withSonarQubeEnv('temporary-sonarqube') {
                    sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll begin /k:\"appdotnet" /d:sonar.host.url="http://192.168.1.22:9000\"/d:sonar.login="sqp_aa9ef265593815051ac4ae889e2af7fc525fcd5d"
                    sh "dotnet build "
                    sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll end"
                }
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


