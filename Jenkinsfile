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

                echo "Website not ready yet. Attempt ${attempt} of 10."
                sleep(time: 3, unit: 'SECONDS')
            }

            if (status != 0) {
                bat 'docker logs devops-demo'
                error 'Website verification failed after 10 attempts.'
            }
        }
    }
}