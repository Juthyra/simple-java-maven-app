pipeline {
    agent any
    options {
      skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh '/opt/apache-maven-3.9.6/bin/mvn -B -DskipTests clean package'
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
                    junit '**/surefire-reports/*.xml'

                    // Publish integration test results
                    step([$class: 'JUnitResultArchiver', junit allowEmptyResults: true, testResults: '**/failsafe-reports/*.xml'])
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
