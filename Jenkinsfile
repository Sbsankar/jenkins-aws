pipeline {
    agent any

    // Define environment variables if needed
    environment {
        // Your SES verified email address for receiving notifications
        EMAIL_RECIPIENT = 'sivabalasankar2002@gmail.com' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code will be automatically checked out by the pipeline job configuration.'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Running the build script...'
                // Execute your shell script (make sure build.sh has execute permissions: chmod +x build.sh)
                sh './build.sh' 
            }
        }
    }

    // --- Post-Build Actions (Email) ---
    post {
        // Runs regardless of whether the build succeeded or failed
        always {
            // Using the emailext step for notification
            emailext (
                to: env.EMAIL_RECIPIENT,
                
                // Subject includes the job name, build number, and final result (SUCCESS/FAILURE)
                subject: "Jenkins Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                
                // HTML body for better formatting in the email
                body: """
                    <h1>Build Notification - ${currentBuild.currentResult}</h1>
                    <p><strong>Job:</strong> ${env.JOB_NAME}</p>
                    <p><strong>Build Number:</strong> ${env.BUILD_NUMBER}</p>
                    <p><strong>Status:</strong> ${currentBuild.currentResult}</p>
                    <p><strong>Commit SHA:</strong> ${env.GIT_COMMIT}</p>
                    
                    <hr>
                    <p>View the full console output here:</p>
                    <p><a href="${env.BUILD_URL}">Build Link on Jenkins</a></p>
                """
            )
        }
    }
}
