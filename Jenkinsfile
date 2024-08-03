pipeline {
    agent { 
       label 'agent2'
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
                sh "docker build . -t dlegare/maintenance"
            }
        }

        stage('push image')
            steps {
                sh "docker login -u deploy-user --password Pass123! 172.16.189.128:8081"
                sh "docker push dlegare/maintenance"
            }
        }
    }
}