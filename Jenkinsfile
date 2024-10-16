pipeline {
    agent { 
       label 'JavaAgent2'
    }

    environment {
        //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
        ARTIFACT = readMavenPom().getArtifactId()
        VERSION = readMavenPom().getVersion()
        NEXUS_PASSWORD = credentials('DEPLOY_USER_PASSWORD')
    }

    stages {
        stage('clean') {
            steps {
                sh 'mvn clean'
            }
        }
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

        stage('Docker Build') {
            steps {
                echo 'Building Image edu.mv/maintenance'
                sh "docker build . -t ${NEXUS_1}/edu.mv/${ARTIFACT}:${VERSION}"
            }
        }

        stage('Push image to Nexus') {
            steps {
                echo 'Login to Nexus'
                sh 'echo ${NEXUS_PASSWORD} | docker login ${NEXUS_1} --username ${NEXUS_DOCKER_USERNAME} --password-stdin'
                sh 'docker push ${NEXUS_1}/edu.mv/${ARTIFACT}:${VERSION}'
            }
        }
        stage ('Deploy to Kubernetes') {
            steps{
                sshagent(credentials : ['minikube-dev-1']) {
                    sh 'ssh -o StrictHostKeyChecking=no ${USER_KUBE_1}@${KUBE_1} uptime'
                    sh 'ssh -v ${USER_KUBE_1}@{KUBE_1}'
                    sh 'minikube kubectl -- apply -f ./config/${ENV_KUBE}/. --namespace=demomaintenance'
                }
            }
        }

    }
}