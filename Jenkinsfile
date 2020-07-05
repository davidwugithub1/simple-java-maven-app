pipeline {
     agent none
//    agent {
//        label 'java-docker-agent'
//    }
//    agent {
//        docker {
//            image 'maven:3-alpine'
//            args '-v /root/.m2:/root/test/.m2'
//            label 'java-docker-agent'
//        }
//    }
    stages {
        stage('Build') {
    agent {
        label 'java-docker-agent'
    }
            steps {
                sh 'mvn -B -DskipTests clean package'
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
