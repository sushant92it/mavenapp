
pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3.8.6"
    }
    environment {
    SQ_CREDS = credentials('sonarqube-creds')
    }
    stages {
        stage('prep') {
            steps {
                git branch: 'main', url: 'https://github.com/psapp1/mavenapp.git'
            }
        }
        stage('build') {
            steps {
               sh 'mvn -f pom.xml -s settings.xml clean package'
               sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=mavenapp -Dsonar.host.url=http://35.193.196.4:9000/ -Dsonar.login=${SQ_CREDS}'
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
