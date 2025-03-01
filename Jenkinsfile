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
                             rm -rf node_modules package-lock.json
                             npm install --force
                             npx serve -s build &
                             sleep 10
                             npx playwright test
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
