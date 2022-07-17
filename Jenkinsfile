pipeline {
    agent any

    tools {nodejs "Node"}

    stages {
        stage('Hello') {
            environment: {
                GITHUB_TOKEN = credentials('jenkins-git')
            }
            steps {
               script { 
                    if (env.BRANCH_NAME === 'main' && env.BRANCH_NAME != 'staging') {
                        echo 'This is not master or staging'
                        sh 'npm run s:release'
                    } else {
                        echo 'things and stuff'
                    }
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
