//node {
//    checkout scm
//
//    docker.withRegistry('', 'docker-hub-id') {
//        def image = docker.image('davidwu93/jenkins:2.0')
//        image.pull()
//    }
//}

pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
//            image 'davidwu93/jenkins-agent:1.0'
//            args '-v /root/.m2:/root/test/.m2'
            label 'java-docker-agent'
        }
    }
    environment {
        registry = "davidwu93/jenkins"
        registryCredential = "docker-hub-id"
    }
    stages {
        stage('Build') {
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
