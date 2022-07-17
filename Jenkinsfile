pipeline {
    agent any

    tools {nodejs "Node"}

    environment {
        GITHUB_TOKEN = credentials('jenkins-git')
    }
    stages {
        stage('Hello') {
            steps {
               script { 
                    echo 'Starting Release'
                    echo ${params.Version}
                    sh 'gh release list'
                    sh 'npm ci'
                    sh 'npm run s:release'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
    }
}
