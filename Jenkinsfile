pipeline {
    agent any
    triggers {
        scm('H/5 * * * *')
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    if (!fileExists('package-lock.json')) {
                        sh 'npm install'
                    } else {
                        sh 'npm ci'
                    }
                }
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
            post {
                failure {
                    mail to: 'your-email@example.com',
                         subject: "Build Failed: ${currentBuild.fullDisplayName}",
                         body: "Something went wrong. Please check the Jenkins logs."
                }
            }
        }
        stage('Deploy to Render') {
            steps {
                script {
                    def renderDeployScript = """
                    #!/bin/bash
                    # Your Render deployment script goes here
                    # Example: render deploy your-service-name
                    """
                    sh renderDeployScript
                }
            }
            post {
                success {
                    slackSend channel: '#VICTOR_IP1', color: 'good', message: "Successfully deployed build ${env.BUILD_ID}. View it at: ${env.BUILD_URL}"
                }
            }
        }
    }
}
