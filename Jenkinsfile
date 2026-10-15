pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Diagnose GitHub Connection') {
            steps {
                echo '===== Windows User ====='
                bat 'whoami'

                echo '===== Network Configuration ====='
                bat 'ipconfig /all'

                echo '===== GitHub DNS Test ====='
                bat 'nslookup github.com'

                echo '===== Git Version ====='
                bat 'git --version'

                echo '===== GitHub Repository Connection Test ====='
                bat 'git ls-remote https://github.com/Rishitha1125/DevOpsDemo.git'

                echo 'GitHub connectivity test completed.'
            }
        }
    }

    post {
        success {
            echo 'GitHub connectivity diagnostic completed successfully.'
        }

        failure {
            echo 'GitHub connectivity diagnostic failed. Check Console Output.'
        }

        always {
            echo "Build Status: ${currentBuild.currentResult}"
            echo "Build Number: ${env.BUILD_NUMBER}"
        }
    }
}