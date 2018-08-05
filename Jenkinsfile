pipeline {
    agent none

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine'
                }
            }
            steps {
                sh 'ls -l'
                sh 'pwd'
                sh 'mvn --version'
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'williamyeh/ansible:alpine3'
                    args '-v $(pwd):/workdir -w /workdir'
                }
            }
            steps {
                sh 'ansible-playbook --version'
            }
        }
    }
}
