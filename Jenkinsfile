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
                sh "docker build . -t ${NEXUS_1}/edu.mv/maintenance:0.0.17"
            }
        }

        stage('Push image to Nexus') {
            steps {
                echo 'Publish Image to Nexus'
                sh "cat nexus.txt | docker login ${NEXUS_1} --username deploy-user --password-stdin"
                sh "docker push ${NEXUS_1}/edu.mv/maintenance:0.0.17"
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