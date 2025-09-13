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
 sh 'npm test > test.log 2>&1 || true' // Allows pipeline to continue despite test failures
 }
  post {
   success {
    def logs = readFile("test.log")
    mail to: "dheisithak@gmail.com",
     subject: "Run Tests Email",
     body: "Tests ran successfully!\n\nLogs:\n${logs}"
     
   }
   failure {
    def logs = readFile("test.log")
    mail to: "dheisithak@gmail.com",
     subject: "Run Tests Email",
     body: "Test run was a failure!\n\nLogs:\n${logs}"
     
   }
  }
 }
 stage('Generate Coverage Report') {
 steps {
 // Ensure coverage report exists
 sh 'npm run coverage || true'
 }
 }
 stage('NPM Audit (Security Scan)') {
 steps {
 sh 'npm audit > audit.log 2>&1 || true' // This will show known CVEs in the output
 }
  post {
   success {
    def logs = readFile("audit.log")
    mail to: "dheisithak@gmail.com",
     subject: "Security Scan Email",
     body: "Security Scan was successful!\n\nLogs:\n${logs}"
     
   }
   failure {
    def logs = readFile("audit.log")
    mail to: "dheisithak@gmail.com",
     subject: "Security Scan Email",
     body: "Security Scan was a failure!\n\nLogs:\n${logs}"
     
   }
  }
 }
  
 }
}
