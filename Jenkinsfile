pipeline {
    agent {
        label 'worker-qa'
    }


    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -DskipTests clean install package'
            }
        }

        stage('Unit test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker image build -t spring-webapp .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'docker image tag spring-webapp strummer77/spring-webapp:latest'
            }
        }

        stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'dockerpwd-id', variable: 'docker-creds')]) {
                    sh 'docker login -u strummer77 -p dckr_pat_jVvU6WWMZv0frJQ_sdVDpdoA4SQ'
                    sh 'docker image push strummer77/spring-webapp:latest'
                }
            }
        }
    }
}
