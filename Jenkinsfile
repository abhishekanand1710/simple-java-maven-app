pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build and SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                // Optionally use a Maven environment you've configured already
                    withMaven(maven:'Maven 3.5') {
                        sh 'mvn -B -DskipTests clean package sonar:sonar'
                    }
                }
            }
        }
    }
}