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
        set -e

        apt-get update -qq
        apt-get install -y wget unzip curl

        echo "🔍 Fetching latest SonarScanner CLI version..."
        LATEST_VERSION=$(curl -s https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/ \
            | grep -oP 'sonar-scanner-cli-\\K[0-9]+\\.[0-9]+\\.[0-9]+\\.[0-9]+(?=-linux\\.zip)' \
            | sort -V | tail -n1)

        echo "✅ Latest version is: $LATEST_VERSION"

        FILENAME="sonar-scanner-cli-$LATEST_VERSION-linux.zip"
        DIRNAME="sonar-scanner-$LATEST_VERSION-linux"

        if [ ! -d "$DIRNAME" ]; then
            echo "⬇️ Downloading $FILENAME..."
            wget --quiet "https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/$FILENAME"
            unzip -q "$FILENAME"
            rm "$FILENAME"
        fi

        export PATH="$PATH:$(pwd)/$DIRNAME/bin"

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
