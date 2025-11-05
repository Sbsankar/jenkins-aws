pipeline {
    agent any

    environment {
        // ID of the credentials stored in Jenkins (from Manage Jenkins -> Credentials)
        CREDENTIAL_ID = 'GMAIL_SMTP_CREDENTIALS' 
        
        // Recipient email address
        EMAIL_RECIPIENT = 'sivabalasankar2002@gmail.com' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checked out successfully.'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Running the build script...'
                // Executes your script, which should now run without errors.
                sh './build.sh' 
            }
        }
    }

    // --- Post-Build Actions (Email) ---
    post {
        // This block runs every time, whether the build succeeds or fails.
        always {
            // Use withCredentials to explicitly load the App Password and Username variables.
            withCredentials([usernamePassword(credentialsId: env.CREDENTIAL_ID, usernameVariable: 'SMTP_USER', passwordVariable: 'SMTP_PASS')]) {
                
                // Use the standard 'mail' step for reliable, explicit SMTP authentication.
                mail (
                    // Gmail Host/Port/Security settings
                    smtpHost: 'smtp.gmail.com',
                    smtpPort: 587,
                    starttls: true,
                    
                    // Use the credentials loaded via withCredentials
                    username: SMTP_USER, 
                    password: SMTP_PASS,

                    to: env.EMAIL_RECIPIENT,
                    subject: "Jenkins Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: "Build Status: ${currentBuild.currentResult}\nJob: ${env.JOB_NAME}\n\nView console output here: ${env.BUILD_URL}"
                )
            }
        }
    }
}
