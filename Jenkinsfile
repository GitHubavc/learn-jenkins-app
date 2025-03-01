pipeline {
    agent any

    stages {
        /***
        stage('Build') {
            agent{
                 docker{
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
        */
        stage('Test'){
            agent{
                 docker{
                            image 'node:18-alpine'
                            reuseNode true
                 }
             }
            steps{
                sh '''
                    #test -f build/index.html
                    npm test
                '''
            }
        }

        stage('E2E'){
                    agent{
                         docker{
                                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                                    args '--ipc=host'
                                    reuseNode true
                         }
                     }
                    steps{
                        sh '''
                             rm -rf node_modules package-lock.json  # Clean up previous installations
                             npm install --force                   # Install dependencies with force
                             npx serve -s build &                  # Start the server in the background
                             sleep 10                               # Give the server time to start
                             npx playwright test                   # Run Playwright tests
                        '''
                    }
                }
    }

    post {
        always{
            junit 'jest-results/junit.xml'
        }
    }
}
