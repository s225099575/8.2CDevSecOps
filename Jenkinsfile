pipeline {
 agent any
 environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }
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
 }
stage('SonarCloud Analysis') {
    steps {
        sh '''
        # Add SonarScanner CLI to PATH
        chmod +x $(pwd)/sonar-scanner-7.2.0.5079-linux-x64/bin/sonar-scanner
        export PATH=$PATH:$(pwd)/sonar-scanner-7.2.0.5079-linux-x64/bin

        # Run SonarScanner
        sonar-scanner \
          -Dsonar.projectKey=s225099575_8.2CDevSecOps \
          -Dsonar.organization=s225099575 \
          -Dsonar.sources=. \
          -Dsonar.exclusions=node_modules/**,test/** \
          -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \
          -Dsonar.login=$SONAR_TOKEN
        '''
    }
}

 }
}
