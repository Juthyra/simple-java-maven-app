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
        // stage('Test') {
        //     steps {
        //         script {
        //             // Unit Testing
        //             sh '/opt/apache-maven-3.9.6/bin/mvn test'

        //             // Integration Testing
        //             sh '/opt/apache-maven-3.9.6/bin/mvn verify -Pintegration-tests'
        //         }
        //     }
        //     post {
        //         always {
        //             // JUnit for Unit Tests
        //             junit '**/surefire-reports/*.xml'

        //             // Publish integration test results
        //             step([$class: 'JUnitResultArchiver', testResults: '**/failsafe-reports/*.xml'])
        //         }
        //     }
        // }
        stage ('Build & Unit test'){
		    steps {
				sh '/opt/apache-maven-3.9.6/bin/mvn clean verify -DskipITs=true';
		      	junit '**/target/surefire-reports/TEST-*.xml'
		      	archiveArtifacts  'target/*.jar'
			}
   	    }
	
	   stage ('Integration Test'){
	        steps {
    			sh  '/opt/apache-maven-3.9.6/bin/mvn clean verify -Dsurefire.skip=true';
				junit '**/target/failsafe-reports/TEST-*.xml'
      			archiveArtifacts  'target/*.jar'
      		}
        }	
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
