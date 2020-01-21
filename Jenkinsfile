pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('SonarQube analysis') {
                withSonarQubeEnv('sonar_scanner') {
                  sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar ' +
                  '-f all/pom.xml ' +
                  '-Dsonar.projectKey=com.huettermann:all:master ' +
                  '-Dsonar.login=$SONAR_UN ' +
                  '-Dsonar.password=$SONAR_PW ' +
                  '-Dsonar.language=java ' +
                  '-Dsonar.sources=. ' +
                  '-Dsonar.tests=. ' +
                  '-Dsonar.test.inclusions=**/*Test*/** ' +
                  '-Dsonar.exclusions=**/*Test*/**'
                }
            }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
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