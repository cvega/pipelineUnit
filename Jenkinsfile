pipeline {

    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    triggers {
        pollSCM('*/5 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('build') {

            steps {
                withEnv(["GRADLE_HOME=${tool name: 'GRADLE_3', type: 'hudson.plugins.gradle.GradleInstallation'}"]) {
                    withEnv(["PATH=${env.PATH}:${env.GRADLE_HOME}/bin"]) {

                        // Checking the env
                        echo "GRADLE_HOME=${env.GRADLE_HOME}"
                        echo "PATH=${env.PATH}"

                        sh 'gradle clean build test -i'
                    }
                }
            }
        }

        stage('validate') {
            steps {
                echo 'TODO: syntactic validation of Jenkinsfiles'
            }
        }
    }

    post {
        always {
            echo 'pipeline unit tests completed'
        }

        success {
            echo 'pipeline unit tests PASSED'
        }

        failure {
            echo 'pipeline unit tests FAILED'
        }

        changed {
            echo 'pipeline unit tests results have CHANGED'
        }

        unstable {
            echo 'pipeline unit tests have gone UNSTABLE'
        }

    }
}
