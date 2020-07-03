pipeline {
    agent {
        label 'java-docker-agent'
    }
//    agent none
    stages {
        stage('Build') {
            agent {
               docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/test/.m2'
//                    label 'java-docker-agent'
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
                sh 'sleep 180'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh 'pwd'
                sh 'cat ./jenkins/scripts/deliver.sh'
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
