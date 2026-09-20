pipeline {

    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Verify WAR') {
            steps {
                bat 'dir target'
            }
        }

        stage('Deploy to Tomcat') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'tomcat-credentials',
                        usernameVariable: 'TOMCAT_USER',
                        passwordVariable: 'TOMCAT_PASSWORD'
                    )
                ]) {

                    bat '''
                    curl -u %TOMCAT_USER%:%TOMCAT_PASSWORD% ^
                    "http://localhost:7080/manager/text/deploy?path=/jenkins-demo&update=true" ^
                    --upload-file target/jenkins-demo.war
                    '''

                }
            }
        }
    }

    post {

        success {
            echo 'Application deployed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }

    }
}