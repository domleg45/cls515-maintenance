pipeline {
    agent { 
       label 'agent2'
    }
    stages {
        stage('build') {
            steps {
                sh 'mvn clean build'
            }
        }
        stage('deploy') {
            steps {
                sh 'mvn clean deploy'
            }
        }
    }
}