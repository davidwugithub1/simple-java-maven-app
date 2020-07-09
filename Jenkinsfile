pipeline {
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
    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-id', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    // some block
                    sh '''
                       set +x
                       echo "my user: " $user " my password: " $pass
                    '''
                }
            }
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
