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
                sh 'mvn clean deploy'
            }
        }
    }
}