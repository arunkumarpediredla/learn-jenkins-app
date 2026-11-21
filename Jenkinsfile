pipeline {
    agent any

    stages {
        stage('Build') {
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

        stage('Test') {
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                    test -s test-results/junit.xml
                '''
            }
        }
        stage ('E2E') {
            steps {
                 sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }
    }
    post {
        always {
            junit testResults: 'test-results/junit.xml', allowEmptyResults: true
        }
    }
}
