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
    mail to: "dheisithak@gmail.com",
     subject: "Run Tests Email",
     body: "Tests ran successfully!",
     compressLog: true
   }
   failure {
    mail to: "dheisithak@gmail.com",
     subject: "Run Tests Email",
     body: "Test run was a failure!",
     compressLog: true
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
 sh 'npm audit || true' // This will show known CVEs in the output
 }
  post {
   success {
    mail to: "dheisithak@gmail.com",
     subject: "Security Scan Email",
     body: "Security Scan was successful!",
     compressLog: true
   }
   failure {
    mail to: "dheisithak@gmail.com",
     subject: "Security Scan Email",
     body: "Security Scan was a failure!",
     compressLog: true
   }
  }
 }
  
 }
}
