pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v $HOME/.m2:/root/.m2'
            reuseNode true
        }
    }
    stages {
        stage('Environment Setup') {
            steps {
                sh 'export MAVEN_OPTS="-Dhttps.protocols=TLSv1.2 -Xmx2048m -Djava.net.preferIPv4Stack=true"'
            }
        }
        stage('Build') {
            steps {
                retry(10) {
                    sh 'mvn -Djavax.net.debug=all -X -e -DskipTests clean package spring-boot:repackage'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -B clean test && mvn jacoco:report'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo deployment unimplemented'
            }
        }
    }
}