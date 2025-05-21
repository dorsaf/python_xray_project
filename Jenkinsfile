pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest -m tests/test_sample.py'
            }
        }

        stage('Upload Results to Xray') {
            steps {
                sh '''
                    curl -u "${JIRA_USER}:{$JIRA_PASSWORD}" \
                         -F "file=@test-results/results.xml" \
                         "{$JIRA_BASE_URL}/rest/raven/1.0/import/execution/junit?projectKey={$JIRA_PROJECT_KEY}"
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/results.xml'
        }
    }
}
