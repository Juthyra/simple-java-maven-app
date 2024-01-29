pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                script {
                    // Unit Testing
                    sh '/opt/apache-maven-3.9.6/bin/mvn test'

                    // Integration Testing
                    sh '/opt/apache-maven-3.9.6/bin/mvn verify -Pintegration-tests'
                }
            }
            post {
                always {
                    // JUnit for Unit Tests
                    junit 'target/surefire-reports/*.xml'

                    // Publish integration test results
                    step([$class: 'JUnitResultArchiver', testResults: '**/target/failsafe-reports/*.xml'])
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
