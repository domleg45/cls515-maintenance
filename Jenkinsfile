pipeline {
    agent { 
       label 'AgentJava'
    }
    stages {
        stage('clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('deploy') {
            steps {
                sh 'mvn clean deploy -s ./settings.xml'
            }
        }

        stage('docker build') {
            steps {
                echo 'Building Image ...'
                sh "docker build . -t 172.16.189.130:8082/edu.mv/maintenance:latest"
            }
        }

        stage('push image') {
            steps {
                sh "docker login -u deploy-user --password todopass 172.16.189.130:8081"
                sh "docker push dlegare/maintenance"
            }
        }
    }
}