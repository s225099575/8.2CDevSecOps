pipeline {
 agent any
 environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }
 stages {
 stage('Checkout') {
 steps {
 git branch: 'main', url: ' https://github.com/s225099575/8.2CDevSecOps.git'
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
 apt-get update
            apt-get install -y wget unzip
 # Download SonarScanner CLI if it doesn't already exist
        if [ ! -d "sonar-scanner-5.12.0.40707-linux" ]; then
            wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.12.0.40707-linux.zip
            unzip sonar-scanner-cli-5.12.0.40707-linux.zip
        fi

        # Add SonarScanner to PATH for this script
        export PATH=$PATH:$(pwd)/sonar-scanner-5.12.0.40707-linux/bin

        # Run SonarScanner with project properties
        sonar-scanner \
          -Dsonar.projectKey=8.2CDevSecOps \
          -Dsonar.organization=your_organization_name \
          -Dsonar.sources=. \
          -Dsonar.exclusions=node_modules/**,test/** \
          -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \
          -Dsonar.login=$SONAR_TOKEN
'''
 }
 }
 }
}
