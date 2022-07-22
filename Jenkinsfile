def REPO_LATEST_TAG = 'initial_value'
def PR_NAME = ''
def URL_PATH = ''

pipeline {
    agent any

    tools {nodejs "Node"}

    environment {
        GITHUB_TOKEN = credentials('jenkins-git')
        JENKINS_TOKEN = credentials('jenkins-remote')
    }
    stages {
        stage('Hello') {
            steps {
               script { 
                    echo 'Starting Release'
                    sh "echo ${params.Version}"
                    sh 'gh release list'
                    // sh 'gh release list -L 1'
                    // REPO_LATEST_TAG = sh(returnStdout: true, script: 'gh release list -L 1')
                    sh "echo ${REPO_LATEST_TAG}"
                    // sh 'npm ci'
                    // sh 'npm run s:release'
                    // REPO_LATEST_TAG=sh(returnStdout: true, script: 'cat .VERSION')
                    REPO_LATEST_TAG=2.1
                    sh "echo Version is ${REPO_LATEST_TAG}"
                    PR_NAME = 'updated-version-${REPO_LATEST_TAG}'
                }
                sh "echo Latest released version is ${REPO_LATEST_TAG}"
            }
        }
        stage('Trigger Job') {
            steps {
                script {
                    sh "echo In Trigger step  ${REPO_LATEST_TAG}"
                    echo 'Triggering remote script to execute job'
                    // sh """curl -X POST -u '$JENKINS_TOKEN_USR:$JENKINS_TOKEN_PSW' http://localhost:9090/job/TestDraftPR/buildWithParameters?token=1234 --data 'VERSION=${REPO_LATEST_TAG}' --data 'PR_NAME=${PR_NAME}'"""
                    //sh "curl -X POST -u '$JENKINS_TOKEN_USR:$JENKINS_TOKEN_PSW' http://localhost:9090/job/TestDraftPR/buildWithParameters?token=1234 --data VERSION=${REPO_LATEST_TAG} --data PR_NAME=${PR_NAME}"
                    //sh('curl -X POST -u $JENKINS_TOKEN_USR:$JENKINS_TOKEN_PSW http://localhost:9090/job/TestDraftPR/buildWithParameters?token=1234 --data VERSION="${REPO_LATEST_TAG}" --data PR_NAME=${PR_NAME}') 
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'jenkins-remote', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                        sh 'echo In Trigger step  "${REPO_LATEST_TAG}"'
                        sh 'echo In Trigger step  REPO_LATEST_TAG'
                        URL_PATH = "-u $USERNAME:$PASSWORD http://localhost:9090/job/TestDraftPR/buildWithParameters?token=1234"
                        sh('echo path is ${URL_PATH}')
                        sh 'curl -X POST -u' $URL_PATH '--data VERSION=' $REPO_LATEST_TAG '
                        //sh 'curl -X POST -u $USERNAME:$PASSWORD http://localhost:9090/job/TestDraftPR/buildWithParameters?token=1234 --data VERSION="${REPO_LATEST_TAG}" --data PR_NAME="${PR_NAME}"'
                    }
                }
            }
        }
    }
}
