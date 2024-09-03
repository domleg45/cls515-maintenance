pipeline {
    agent { 
       label 'JavaAgent'
    }

    environment {
        //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
        IMAGE = readMavenPom().getArtifactId()
        VERSION = readMavenPom().getVersion()
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
                sh "docker build . -t ${NEXUS_1}/edu.mv/maintenance:${VERSION}"
            }
        }

        stage('Push image to Nexus') {
            steps {
                echo 'Publish Image to Nexus'
                sh "cat nexus.txt | docker login ${NEXUS_1} --username deploy-user --password-stdin"
                sh "docker push ${NEXUS_1}/edu.mv/maintenance:${VERSION}"
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