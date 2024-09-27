pipeline {
    agent { 
       label 'JavaAgent2'
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
                sh 'mvn install'
            }
        }

        stage('docker build') {
            steps {
                echo 'Building Image edu.mv/maintenance'
                sh "docker build . -t ${NEXUS_1}/edu.mv/maintenance:${VERSION}"
            }
        }

        stage('Push image to Nexus') {
            steps {
                echo 'Publish Image to Nexus ${NEXUS_1}'
                sh "echo ${NEXUS_DOCKER_PASSWORD} | docker login ${NEXUS_1} --username ${NEXUS_DOCKER_USERNAME} --password-stdin"
                sh "docker push ${NEXUS_1}/edu.mv/maintenance:${VERSION}"
            }
        }

        stage('Deploy Image to Minikube') {
            steps {
                echo 'Deploy image to minikube env ${ENV_KUBE}'
                sh "minikube kubectl -- apply -f ./config/${ENV_KUBE}/. --namespace=demomaintenance"
            }
        }
    }
}