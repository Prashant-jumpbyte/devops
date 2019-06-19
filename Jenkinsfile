pipeline {
    agent any
    stages {
        stage('Build Application'){
            steps {
                sh "mvn clean compile"
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
        stage('Build Image Docker'){
            steps {
                sh "docker build -t EventManager ."
            }
        }
        stage('Deploy'){
            steps {
                sh "docker rm -f EventManagerAPI"
                sh "docker run --name EventManagerAPI -p 50001:9090 -d EventManager"
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
