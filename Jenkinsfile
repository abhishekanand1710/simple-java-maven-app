// pipeline {
//     agent {
//         docker {
//             image 'maven:3-alpine'
//             args '-v /root/.m2:/root/.m2'
//         }
//     }
// //     stages {
// //         stage('Build and SonarQube analysis') {
// //             steps {
// //                 withSonarQubeEnv('SonarQube') {
// //                 // Optionally use a Maven environment you've configured already
// //                     withMaven(maven:'Maven 3.5') {
// //                         sh 'mvn -B -DskipTests clean package sonar:sonar'
// //                     }
// //                 }
// //             }
// //         }
// //     }
// }

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
        stage('SoanrQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
                }
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