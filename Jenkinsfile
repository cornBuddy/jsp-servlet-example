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
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'williamyeh/ansible:alpine3'
                    args '-v /usr/bin/docker:/usr/bin/docker \
                          -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                sh 'echo $PATH'
                sh 'docker run hello-world'
                sh 'ls -l'
                sh 'pwd'
            }
        }
    }
}
