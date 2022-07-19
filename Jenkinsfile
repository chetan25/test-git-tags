pipeline {
    agent any

    tools {nodejs "Node"}

    environment {
        GITHUB_TOKEN = credentials('jenkins-git')
        BUILD_TAG = 'weew'
    }
    stages {
        stage('Hello') {
            steps {
               script { 
                    echo 'Starting Release'
                    sh "echo ${params.Version}"
                    sh 'gh release list'
                    sh 'gh release list -L 1'
                    sh "gh release list -L 1 > env.BUILD_TAG"
                    sh "echo ${env.BUILD_TAG}"
                    // sh 'npm ci'
                    // sh 'npm run s:release'
                }
                sh "echo ${env.BUILD_TAG}"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
    }
}
