

pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
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
                echo 'Docker deployment completed'
            }
        }
    }

    post {
        always {
            emailext(
                to: 'rishithaseethana@gmail.com',
                subject: "Jenkins ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """Hello,

Pipeline execution has finished.

Project: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build Status: ${currentBuild.currentResult}

Please check Jenkins Console Output for details.
"""
            )
        }
    }
}
