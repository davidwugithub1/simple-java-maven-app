pipeline {
//    agent {
//        label 'java-docker-agent'
//    }
    agent none
    stages {
        stage('Build') {
            agent {
//               docker {
//                    image 'maven:3-alpine'
//                    args '-v /root/.m2:/root/test/.m2'
//                    label 'java-docker-agent'
//                }
                label 'java-docker-agent'
                args '-v /root/.m2:/root/test/.m2'
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            agent {
                label 'java-docker-agent'
            }
            options {
                timeout(time: 120, unit: 'SECONDS')
                retry(3) 
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
            agent {
                label 'java-docker-agent'
                args '-v /root/.m2:/root/test/.m2'
            }
            steps {
                sh 'pwd'
                sh 'cat ./jenkins/scripts/deliver.sh'
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
