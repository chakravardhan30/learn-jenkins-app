pipeline {
    agent any

    stages {
        stage('Build') {
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
        }

        stage('check index.html')
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
        }
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
                npm run tests
                '''
            }

        }
        
    }
}
