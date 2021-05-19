pipeline {
    agent any
    
    options {
    	withAWS(credentials: 'S3 Access')
    }

    stages {
        stage('Checkout') {
            steps {                
                git 'https://github.com/mozaic-services/javascript-calculator'
            }
        }
        stage('Deploy to Test') {
            steps {                
                s3Upload(
                    bucket: env.S3_BUCKET_TEST,
                    includePathPattern: '**/*',
                    excludePathPattern: 'Jenkinsfile'
                )
            }
        }

        stage('Wait for approval') {
            snDevOpsChange()
        }

        stage('Deploy to Production') {
            steps {
                snDevOpsChange()

                s3Upload(
                    bucket: env.S3_BUCKET_PRODUCTION,
                    includePathPattern: '**/*',
                    excludePathPattern: 'Jenkinsfile'
                )
            }
        }
    }
}
