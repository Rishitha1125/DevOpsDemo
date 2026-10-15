pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                echo 'Source code fetched from GitHub successfully.'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker --version'
                bat 'docker build -t devops-demo:latest .'
                echo 'Docker image built successfully.'
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
                echo 'Docker container started.'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps --filter name=devops-demo'

                script {
                    int status = 1

                    for (int attempt = 1; attempt <= 10; attempt++) {
                        status = bat(
                            returnStatus: true,
                            script: 'curl.exe --fail --silent --show-error http://127.0.0.1:8081/'
                        )

                        if (status == 0) {
                            echo 'Website deployment verified successfully.'
                            break
                        }

                        echo "Website not ready. Attempt ${attempt} of 10."
                        sleep(time: 3, unit: 'SECONDS')
                    }

                    if (status != 0) {
                        bat(
                            returnStatus: true,
                            script: 'docker logs devops-demo'
                        )
                        error 'Website verification failed after 10 attempts.'
                    }
                }
            }
        }
    }

  
post {
    success {
        echo 'All pipeline stages completed successfully.'
    }

    failure {
        echo 'Pipeline failed. Check Console Output for details.'
    }

    always {
        script {
            echo "Build result: ${currentBuild.currentResult}"

            emailext(
                to: 'rishithaseethana@gmail.com',
                subject: "Jenkins ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                mimeType: 'text/plain',
                body: """Hello,

Jenkins pipeline execution has finished.

Project: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build Status: ${currentBuild.currentResult}

Website URL: http://localhost:8081/
Build URL: ${env.BUILD_URL}
"""
            )
        }
    }
}
