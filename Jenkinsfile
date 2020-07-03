pipeline {
//    agent {
//        label 'java-docker-agent'
//    }
    agent none
    stages {
        stage('Build') {
            agent {
               docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/test/.m2'
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            agent {
               docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/test/.m2'
                }
            }
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
