// https://jenkins.io/doc/book/pipeline/jenkinsfile/
// build in a maven container
// leave tests for a while
// send builded on the first step war file to all tomcat instances
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'building...'
            }
        }

        stage('Test') {
            steps {
                echo 'testing...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'deploying...'
            }
        }
    }
}
