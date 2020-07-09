node {
    checkout scm

    //docker.withRegistry('https://hub.docker.com/', 'docker-hub-id') {
    docker.withRegistry('', 'docker-hub-id') {
        def image = docker.image('davidwu93/jenkins:2.0')
        image.pull()
    }
//}

//pipeline {
    agent none
//    agent {
//        label 'java-docker-agent'
//    }
//    agent {
//        docker {
//            image 'maven:3-alpine'
//            image 'davidwu93/jenkins-agent:1.0'
//            args '-v /root/.m2:/root/test/.m2'
//            label 'java-docker-agent'
//        }
//    }
    environment {
    registry = "davidwu93/jenkins"
    registryCredential = "Docker hub ID"
  }
    stages {
        stage('Build') {
            agent {
                docker {
                    //image 'maven:3-alpine'
                    //image 'davidwu93/jenkins-agent:1.0'
                    image 'davidwu93/jenkins:2.0'
                    //args '-v /tmp/jenkins/.m2:/root/test/.m2'
                    label 'maven-build'
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
                    label 'maven-build'
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
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/test/.m2'
                    label 'maven-build'
                }
            }
            steps {
                sh 'pwd'
                sh 'cat ./jenkins/scripts/deliver.sh'
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
