 pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11' 
            args ' -v /home/.m2:/root/.m2 -e HTTP_PROXY="http://209.58.84.119:3128" -e HTTPS_PROXY="http://209.58.84.119:3128" --dns 8.8.8.8 '
        }
    }
options {
        skipStagesAfterUnstable()
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
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}