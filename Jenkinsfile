def gv

//CODE_CHANGES = getGitChanges()
pipeline {
    agent any
    /*environment {
        NEW_VERSION = '1.3.0'
        SERVER_CREDENTIALS = credentials('server-credentials')
    }*/
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTest', defaultValue: true, description: '')
    }
    stages {
        stage('init') {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage('build') {
            when {
                expression {
                    BRANCH_NAME == 'master'// && CODE_CHANGES == true
                }
            }
            steps {
                //echo 'Building the application...'
                //echo "Building version ${params.VERSION}"
                script {
                    gv.buildApp()
                }
            }
        }
        stage('test') {
            when {
                expression {
                    params.executeTest && BRANCH_NAME == 'master'
                }
            }
            steps {
                script {
                    gv.testApp()
                }
                //echo 'Testing the application...'
            }
        }
        stage('deploy') {
            steps {
                script {
                    gv.deployApp()
                }
                //echo 'Deploying the application...'
                //echo "Deploying version ${params.VERSION}"
                /*echo "Deploying with ${SERVER_CREDENTIALS}"
                sh ${SERVER_CREDENTIALS}*/
                /*withCredentials([
                    usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD)
                ]) {
                    sh "some script ${USER} ${pwd}"
                }*/
            }
        }
    }
    /*post {
        always {
            // e.g. Sending email
        }
        success {
            // Only on success
        }
        failure {
            // Only on failure
        }
    }*/
}