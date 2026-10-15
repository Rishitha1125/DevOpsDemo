pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                echo 'Source code fetched from GitHub'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker --version'
                bat 'docker build -t devops-demo:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    bat(
                        returnStatus: true,
                        script: 'docker rm -f devops-demo'
                    )
                }

                bat 'docker run -d --name devops-demo -p 8081:8000 devops-demo:latest'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps --filter name=devops-demo'
                bat 'curl.exe --fail http://localhost:8081/'
                echo 'Website deployment verified'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }

        always {
            emailext(
                to: 'rishithaseethana@gmail.com',
                subject: "Jenkins ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """Hello,

Jenkins pipeline execution has finished.

Project: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build Status: ${currentBuild.currentResult}

Check Jenkins Console Output for details.
"""
            )
        }
    }
}