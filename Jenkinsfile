pipeline {
    agent any

    environment {
        // Recipient email address
        EMAIL_RECIPIENT = 'sivabalasankar2002@gmail.com' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checked out.'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Running the build script...'
                sh './build.sh' 
            }
        }
    }

    // --- Post-Build Actions (Email) ---
    post {
        always {
            // This 'mail' step relies ONLY on the successful configuration you saved
            // in "Manage Jenkins -> Configure System" (using Gmail SMTP).
            mail (
                to: env.EMAIL_RECIPIENT,
                subject: "Jenkins Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build Status: ${currentBuild.currentResult}\nJob: ${env.JOB_NAME}\n\nView console output here: ${env.BUILD_URL}"
            )
        }
    }
}
