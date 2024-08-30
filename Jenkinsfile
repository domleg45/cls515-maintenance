pipeline {
    agent { 
       label 'JavaAgent'
    }
    stages {
        stage('clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('docker build') {
            steps {
                echo 'Building Image ...'
                sh "docker build . -t 172.16.189.130:8082/edu.mv/maintenance:0.0.10"
            }
        }

        stage('Push image to Nexus') {
            steps {
                echo 'Publish Image to Nexus'
                sh "cat nexus.txt | docker login 172.16.189.130:8082 --username deploy-user --password-stdin"
                sh "docker push 172.16.189.130:8082/edu.mv/maintenance:0.0.10"
            }
        }

        stage('Deploy Image to Minikube') {
            steps {
                echo 'Deploy image to minikube'
                sh "minikube kubectl -- apply -f . --namespace=demomaintenance"
            }
        }
    }
}