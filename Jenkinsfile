pipeline {
    agent any

    environment {
        SLACK_WEBHOOK_URL = credentials('slack-webhook')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Bonjour et bon courage dans votre projet en DevOps'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*', fingerprint: true
            }
        }
    }

    post {
        success {
            script {
                def msg = "✅ Jenkins Pipeline SUCCESS\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}"
                sh(returnStatus: true, script: """
                    curl -sS -X POST -H 'Content-type: application/json' \
                    --data '{"text":"${msg.replace('"','\\\\\\"")}"}' \
                    "${SLACK_WEBHOOK_URL}"
                """)
            }
        }

        failure {
            script {
                def msg = "❌ Jenkins Pipeline FAILED\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}"
                sh(returnStatus: true, script: """
                    curl -sS -X POST -H 'Content-type: application/json' \
                    --data '{"text":"${msg.replace('"','\\\\\\"")}"}' \
                    "${SLACK_WEBHOOK_URL}"
                """)
            }
        }
    }
}
