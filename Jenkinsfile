pipeline {
    agent any
    
    options {
    	withAWS(credentials: 'S3 Access')
    }

    stages {
        stage('Checkout') {
            snDevOpsStep()
            steps {
                git 'https://github.com/mozaic-services/javascript-calculator'
            }
        }
        stage('Deploy to Test') {
            snDevOpsStep()
            steps {
                s3Upload(
                    bucket: env.S3_BUCKET_TEST,
                    includePathPattern: '**/*',
                    excludePathPattern: 'Jenkinsfile'
                )
            }
        }

        stage('Wait for approval') {
            snDevOpsStep()
            snDevOpsChange()
        }

        stage('Deploy to Production') {
            snDevOpsChange()
            steps {
                s3Upload(
                    bucket: env.S3_BUCKET_PRODUCTION,
                    includePathPattern: '**/*',
                    excludePathPattern: 'Jenkinsfile'
                )
            }
        }
    }
}
