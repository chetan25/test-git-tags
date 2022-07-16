pipeline {
    agent any

    tools {nodejs "Node"}

    stages {
        stage('Hello') {
            steps {
               script { 
                    if (env.BRANCH_NAME != 'master' && env.BRANCH_NAME != 'staging') {
                        echo 'This is not master or staging'
                    } else {
                        echo 'things and stuff'
                    }
                }
            }
        }
    }
}
