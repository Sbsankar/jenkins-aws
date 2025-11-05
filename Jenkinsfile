pipeline {
    agent any // Run on any available Jenkins agent

    stages {
        stage('Build') {
            steps {
                echo 'Running the build script...'
                sh './build.sh'
            }
        }
    }

    post {
        // This 'post' block runs after all stages
        always {
            // Send an email regardless of build status
            emailext (
                to: 'sbsankar215414@gmail.com', // <-- CHANGE THIS
                subject: "Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<p>Build Status: ${currentBuild.currentResult}</p>
                       <p>Check console output at: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                       <pre>${currentBuild.getRawBuild().getLog(100).join('\n')}</pre>"""
            )
        }
    }
}
