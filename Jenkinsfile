pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch name to build')
        string(name: 'VERSION', defaultValue: 'latest', description: 'Version number')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: "${params.BRANCH_NAME}"]],
                          userRemoteConfigs: [[url: 'https://github.com/PiyushKotadiya/test.git']]
                ])
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building branch ${params.BRANCH_NAME} with version ${params.VERSION}"
                    sh 'echo "Hello World" > hello.txt'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    echo -e 'FROM alpine\nRUN echo "Hello World" > /hello.txt\nCMD cat /hello.txt' > Dockerfile
                    docker build -t helloworld:${params.VERSION} .
                    '''
                }
            }
        }
    }

    triggers {
        cron('H 0 * * *') // Run daily at midnight
    }
}
