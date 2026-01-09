pipeline {
    agent any

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
            sh '''
            curl -X POST -H 'Content-type: application/json' \
            --data "{\"text\":\"✅ Jenkins Pipeline SUCCESS\\nJob: ${JOB_NAME}\\nBuild: #${BUILD_NUMBER}\"}" \
            $SLACK_WEBHOOK_URL
            '''
        }

        failure {
            sh '''
            curl -X POST -H 'Content-type: application/json' \
            --data "{\"text\":\"❌ Jenkins Pipeline FAILED\\nJob: ${JOB_NAME}\\nBuild: #${BUILD_NUMBER}\"}" \
            $SLACK_WEBHOOK_URL
            '''
        }
    }
}
