pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven"
    }
	checkout scm
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jgrisel/JpetStore6.git'

                // Run Maven on a Unix agent.
                bat "mvn clean install -Dlicense.skip=true"

                bat "mvn sonar:sonar -Dsonar.login=sqa_f5661fb7af208b22ef5a58598e16ee584d796283"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
