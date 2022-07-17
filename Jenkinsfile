pipeline {
    agent any

    tools {nodejs "Node"}

    environment {
        GITHUB_TOKEN = credentials('jenkins-git')
    }
    stages {
        stage('Hello') {
            steps {
               git url: "https://github.com/chetan25/test-git-tags.git", branch: 'main'
               script { 
                    echo 'Starting Release'
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
