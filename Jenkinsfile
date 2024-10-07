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
        stage('compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Push image to Nexus') {
            steps {
                echo 'Login to Nexus ${NEXUS_1}'
                sh "echo ${ADMIN_PASSWORD} >> pass.txt"
                sh "cat pass.txt | docker login ${NEXUS_1} --username ${ADMIN_USERNAME} --password-stdin"
            }
        }

    }
}