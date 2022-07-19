pipeline {
    agent any

    tools {nodejs "Node"}

    environment {
        GITHUB_TOKEN = credentials('jenkins-git')
        BUILD_TAG = ''
    }
    stages {
        stage('Hello') {
            steps {
               script { 
                    echo 'Starting Release'
                    sh "echo ${params.Version}"
                    sh 'gh release list'
                    sh "gh release list -L 1 > env.BUILD_TAG"
                    sh "echo ${env.BUILD_TAG}"
                    // sh 'npm ci'
                    // sh 'npm run s:release'
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
