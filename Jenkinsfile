pipeline {
    agent any

    // Define environment variables if needed, though $GIT_COMMIT, $BUILD_NUMBER, etc. are automatic
    environment {
        // You can add your own variables here if necessary
    }

    stages {
        stage('Checkout') {
            steps {
                // The pipeline automatically checks out the repository defined in the job configuration
                echo 'Source code checked out.'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Running the build script...'
                // Execute your shell script
                sh './build.sh' 
            }
        }
    }

    // --- Post-Build Actions (Email) ---
    post {
        // This 'always' block runs regardless of whether the build succeeded or failed
        always {
            // Check if the Extended Email Plugin (emailext) is available before trying to send
            script {
                if (manager.has = currentBuild.rawBuild.getAction(hudson.plugins.emailext.ExtendedEmailPublisher.class)) {
                    emailext (
                        to: 'sbsankar215414@gmail.com', // **<-- CHANGE THIS TO YOUR VERIFIED EMAIL**
                        subject: "Jenkins Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        
                        // Simple body with key variables and a link to the console output
                        body: """
                            <h1>Build Notification - ${currentBuild.currentResult}</h1>
                            <p><strong>Job:</strong> ${env.JOB_NAME}</p>
                            <p><strong>Build Number:</strong> ${env.BUILD_NUMBER}</p>
                            <p><strong>Status:</strong> ${currentBuild.currentResult}</p>
                            <p><strong>Commit:</strong> ${env.GIT_COMMIT}</p>
                            
                            <hr>
                            <p>View the full console output here:</p>
                            <p><a href="${env.BUILD_URL}">Build Link on Jenkins</a></p>
                        """
                    )
                } else {
                    echo "Extended Email Publisher not configured or plugin not installed."
                }
            }
        }
    }
}
