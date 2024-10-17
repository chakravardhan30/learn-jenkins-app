pipeline {
    agent any

    stages {
       /* stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                ls -la
                npm --version
                node --version
                npm ci
                npm run build
                ls -la

                '''
            }
        }*/

       /* stage('check index.html')
        {
            steps{
                 script {
                    if (fileExists('build/index.html')) {
                        echo 'index.html exists'
                        
                        def indexHtmlContent = readFile('build/index.html')
                        echo "Contents of index.html:\n${indexHtmlContent}"
                    } else {
                        echo 'index.html does not exist'
                        error 'Build failed: index.html is missing in the build folder.'
                    }
                }
            }
        }*/
        stage('Test')
        {
            agent {
                docker {
                    image 'node:18-alpine'  
                    reuseNode true
                }
            }

            steps{
                sh '''
                # echo 'running tests'
                npm run test
                '''
            }

        }

        stage('E2E')
        {
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh'''

                npm install serve
                node_modules/.bin/serve -s build &
                sleep 10
                npx playwright test --reporter=html
                '''
            }
        }
        
    }
    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}
