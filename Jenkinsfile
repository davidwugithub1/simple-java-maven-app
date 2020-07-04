pipeline {
//    agent {
//        label 'java-docker-agent'
//    }
    agent none
    stages {
        stage('Build') {
            agent {
               docker {
//                    image 'maven:3-alpine'
//                    args '-v /root/.m2:/root/test/.m2 --privileged'
                    label 'java-docker-agent'
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}
