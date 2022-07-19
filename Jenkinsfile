def BUILD_TAG = 'initial_value'
pipeline {
    agent any

    tools {nodejs "Node"}

    environment {
        GITHUB_TOKEN = credentials('jenkins-git')
        JENKINS_REMOTE_USER = 
    }
    stages {
        stage('Hello') {
            steps {
               script { 
                    echo 'Starting Release'
                    sh "echo ${params.Version}"
                    sh 'gh release list'
                    sh 'gh release list -L 1'
                    BUILD_TAG = sh(returnStdout: true, script: 'gh release list -L 1')
                    sh "echo ${BUILD_TAG}"
                    // sh 'npm ci'
                    // sh 'npm run s:release'
                }
                sh "echo ${BUILD_TAG}"
            }
        }
        stage('Trigger Job') {
            steps {
                echo 'Triggering remote script to execute job'
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'jenkins-remote', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                   sh 'sh curl -u $USERNAME:$PASSWORD http://localhost:9090/job/TestDraftPR/buildWithParameters?token=1234&VERSION=2.1'
                }
            }
        }
    }
}
