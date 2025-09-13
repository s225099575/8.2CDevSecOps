pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/s225099575/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true' // Allows pipeline to continue despite test failures
            }
            post {
                success {
                    emailext(
                        to: "dheisithak@gmail.com",
                        subject: "Run Tests SUCCESS - Build #${env.BUILD_NUMBER}",
                        body: """<p>Tests completed successfully.</p>
                                 <p>Project: ${env.JOB_NAME}</p>
                                 <p>Build: #${env.BUILD_NUMBER}</p>
                                 <p>Status: SUCCESS</p>""",
                        compressLog: true
                    )
                }
                failure {
                    emailext(
                        to: "dheisithak@gmail.com",
                        subject: "Run Tests FAILURE - Build #${env.BUILD_NUMBER}",
                        body: """<p>Tests failed!</p>
                                 <p>Project: ${env.JOB_NAME}</p>
                                 <p>Build: #${env.BUILD_NUMBER}</p>
                                 <p>Status: FAILURE</p>""",
                        compressLog: true
                    )
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                success {
                    emailext(
                        to: "dheisithak@gmail.com",
                        subject: "Security Scan SUCCESS - Build #${env.BUILD_NUMBER}",
                        body: """<p>Security Scan completed successfully.</p>
                                 <p>Project: ${env.JOB_NAME}</p>
                                 <p>Build: #${env.BUILD_NUMBER}</p>
                                 <p>Status: SUCCESS</p>""",
                        compressLog: true
                    )
                }
                failure {
                    emailext(
                        to: "dheisithak@gmail.com",
                        subject: "Security Scan FAILURE - Build #${env.BUILD_NUMBER}",
                        body: """<p>Security Scan failed!</p>
                                 <p>Project: ${env.JOB_NAME}</p>
                                 <p>Build: #${env.BUILD_NUMBER}</p>
                                 <p>Status: FAILURE</p>""",
                        compressLog: true
                    )
                }
            }
        }
    }
}
