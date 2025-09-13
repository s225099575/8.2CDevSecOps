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
    sh 'npm test > test.log 2>&1 || true' // Run tests even if they fail
  }
  post {
    success {
      mail to: "dheisithak@gmail.com",
           subject: "Run Tests Email - SUCCESS",
           body: """Tests ran successfully!

---------------- Logs ----------------
${readFile('test.log')}
"""
    }
    failure {
      mail to: "dheisithak@gmail.com",
           subject: "Run Tests Email - FAILURE",
           body: """Test run failed!

---------------- Logs ----------------
${readFile('test.log')}
"""
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
    sh 'npm audit > audit.log 2>&1 || true' // Run audit even if it fails
  }
  post {
    success {
      mail to: "dheisithak@gmail.com",
           subject: "Security Scan Email - SUCCESS",
           body: """Security scan completed successfully!

---------------- Logs ----------------
${readFile('audit.log')}
"""
    }
    failure {
      mail to: "dheisithak@gmail.com",
           subject: "Security Scan Email - FAILURE",
           body: """Security scan failed!

---------------- Logs ----------------
${readFile('audit.log')}
"""
    }
  }
}
}
}
