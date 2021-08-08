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
                withSonarQubeEnv('SQ') {
                    // Optionally use a Maven environment you've configured already
                    withMaven(maven:'Maven 3.7') {
                        sh 'mvn clean package sonar:sonar'
                    }
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
