pipeline {
    agent any
    
    options {
    	withAWS(credentials: 'S3 Access')
    }

    stages {
        stage('Checkout') {
            steps {
                snDevOpsStep()
                git 'https://github.com/mozaic-services/javascript-calculator'
            }
        }
        stage('Deploy to Test') {
            steps {
                snDevOpsStep()
                s3Upload(
                    bucket: env.S3_BUCKET_TEST,
                    includePathPattern: '**/*',
                    excludePathPattern: 'Jenkinsfile'
                )
            }
        }

        stage('Wait for approval') {
            steps {
                snDevOpsStep()
                snDevOpsChange()
            }
        }

        stage('Deploy to Production') {
            steps {
                snDevOpsStep()
                s3Upload(
                    bucket: env.S3_BUCKET_PRODUCTION,
                    includePathPattern: '**/*',
                    excludePathPattern: 'Jenkinsfile'
                )
            }
        }
    }
}
