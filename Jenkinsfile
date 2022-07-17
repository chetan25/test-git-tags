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
                    sh 'git branch: 'main', url: "https://github.com/chetan25/test-git-tags.git"'
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
