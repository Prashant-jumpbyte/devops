pipeline {
    agent any
    stages {
        stage('Build'){
            steps {
                sh "mvn clean compile"
            }
        }
        
        
        stage('Sonarqube') {

            environment {

                scannerHome = tool 'SonarQubeScanner'

            }

            steps {

                withSonarQubeEnv('sonarqube') {

                    sh "${scannerHome}/bin/sonar-scanner"

                }

                timeout(time: 10, unit: 'MINUTES') {

                    waitForQualityGate abortPipeline: true

                }

            }

        }
        
        
        
        stage('Tests'){
            steps {
                sh "mvn test"
            }
        }
        stage('Package'){
            steps {
                sh "mvn install -DskipTests"
            }
        }
    }

    post {
        cleanup {
            // leave workspace clean after build
            archiveArtifacts 'target/*.jar'
            junit 'target/surefire-reports/*.xml'
            cleanWs()
        }
    }
}
