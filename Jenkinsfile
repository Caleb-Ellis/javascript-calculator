pipeline {
    agent any
    
    options {
    	withAWS(credentials: 'S3 Access')
    }

    stages {
        stage('Checkout') {
            steps {
                snDevOpsStep(enabled:true)
                
                git 'https://github.com/mozaic-services/javascript-calculator'
            }
        }
        stage('Deploy to Test') {
            steps {
                snDevOpsStep(enabled:true)
                
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
