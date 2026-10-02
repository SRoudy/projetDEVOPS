

pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {

        stage('git') {
            steps {
                git branch: 'ma-branche',
                    url: 'https://github.com/SRoudy/projetDEVOPS.git'
            }
        }

        stage('Maven Clean') {
            steps {
                dir('backend') {
                    sh 'mvn clean'
                }
            }
        }

        stage('Maven Compile') {
            steps {
                dir('backend') {
                    sh 'mvn compile'
                }
            }
        }

        stage('Maven Test') {
            steps {
                dir('backend') {
                    sh 'mvn test'
                }
            }
        }

        stage('Maven Package') {
            steps {
                dir('backend') {
                    sh 'mvn package'
                }
            }
        }

        stage('SonarQube') {
            steps {
                dir('backend') {
                    withCredentials([
                        string(
                            credentialsId: 'sonar-token',
                            variable: 'SONAR_TOKEN'
                        )
                    ]) {
                        sh '''
                            mvn org.sonarsource.scanner.maven:sonar-maven-plugin:5.5.0.6356:sonar -Dsonar.projectKey=projetDEVOPS -Dsonar.host.url=http://localhost:9000 -Dsonar.token=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
            echo 'Pipeline exécuté avec succès !'
        }

        failure {
            echo 'Le pipeline a échoué : consulte la Console Output.'
        }

        always {
            echo 'Fin de l’exécution du pipeline.'
        }
    }
}
