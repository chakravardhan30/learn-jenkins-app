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
                if(fileExists('build/index.html'))
                {
                    echo 'index.html exists'
                    def indexHtmlContent = readFile('build/index.html')
                        echo "Contents of index.html:\n${indexHtmlContent}"
                }
                else{
                    echo 'index.html doesnt exists'
                }
            }
        }
        stage('Test')
        {
            steps{
                sh '''
                npm run tests
                '''
            }

        }
        
    }
}
