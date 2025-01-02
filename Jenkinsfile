pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws_access_key')
        AWS_SECRET_ACCESS_KEY = credentials('aws_secret_access_key')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci 
                    npm run build
                    ls -la
                '''
            }
        }

        // stage('Test') {
        //     agent {
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh '''
        //             test -f build/index.html
        //             npm test
        //         '''
        //     }
        // }

        stage('Deploy'){
            agent {
                docker {
                    image 'amazon/aws-cli:2.4.11'
                    args '--entrypoint='
                    reuseNode true
                }
            }
            steps {
                sh '''
                    aws s3 ls
                '''
            }
        }

    }
    post {
            always {
                junit 'test-results/junit.xml'
            }
        }
}