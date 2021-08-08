pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('SCM') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/billal-ayyoob/java-maven-app.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Analysis with SonarQube') {
            steps {
                withSonarQubeEnv(credentialsId: 'f225455e-ea59-40fa-8af7-08176e86507a',
                installationName: 'SQ') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:4.6.2.2472:sonar'
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
