pipeline {
    agent {
        node {
            label 'dockerhost-build-server'
        }
    }

    tools {
        maven 'maven-3.9.6'
    }

    stages {

        stage('Packaging') {
            steps {
                echo 'Packaging...'
                sh 'mvn clean package'
            }
        }

        stage('Cleanup old container') {
            steps {
                echo 'Removing old container if exists...'
                sh 'docker rm -f campaign-demo-server || true'
            }
        }

        stage('cleanup') {
            steps {
                sh 'docker system prune -a --volumes --force --filter "label=campaign-demo-server"'
            }
        }

        stage('build image') {
            steps {
                sh 'docker build -t JAGO-DEV-2026/campaign-demo:v1 --label campaign-demo-server .'
            }
        }

        stage('run container') {
            steps {
                sh '''
                docker run -d \
                  --name campaign-demo-server \
                  --label campaign-demo-server \
                  -p 5000:5000 \
                  JAGO-DEV-2026/campaign-demo:v1
                '''
            }
        }
    }
}
