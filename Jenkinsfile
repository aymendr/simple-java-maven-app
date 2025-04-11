pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven399"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/aymendr/simple-java-maven-app.git'
                
                //bat 'echo Build Project on dev branch'
                // Run Maven on a Unix agent.
                bat "mvn -Dmaven.test.failure.ignore=true clean install"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
                

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    // archiveArtifacts 'target/*.jar'
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stage("Nexus"){
            steps {
                nexusArtifactUploader artifacts:
                [[
                    artifactId: 'my-app', 
                    classifier: '', 
                    file: 'target/my-app-2.0.jar', 
                    type: 'jar'
                ]], 

                credentialsId: 'nexus_credentials', 
                groupId: 'com.mycompany.app', 
                nexusUrl: 'localhost:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'maven-releases', 
                version: '2.0'

            }
        }
    }
}
