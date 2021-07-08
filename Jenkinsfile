pipeline {
    agent {label 'agent' }
    tools { nodejs 'NodeJS'}

    environment {
        CI = 'true'
        registry = 'docker-services-training/aleesha/'
        }
    stages {
        stage('Clone') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Unit test') {
            steps {
                sh 'npm test'
                }
                post {
                    always {
                        junit 'target/surefire-reports/*.xml'
                    }                
                }
            }
        }
        stage('Sonarqube analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonar'){
                        sh 'mvn sonar:sonar -DskipTests'
                     }
                 }
            }
        }
//         stage('Artifactory') {
//             steps {
//                 script {
//                     unstash name:"artifact"
//                     docker.withTool('docker') {
//                         docker.withRegistry('https://artifactory.dagility.com', 'aleesha-registry'){
//                             docker.build(registry + "nodejs:latest").push()
//                         }
//                     }
//                 }
//             }
//         }
    }
}
