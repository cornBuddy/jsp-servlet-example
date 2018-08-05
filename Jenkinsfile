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
                    image 'williamyeh/ansible:debian9'
                    args '-v /usr/bin/docker:/usr/bin/docker \
                          -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                sh 'apt-get upgrade -y'
                sh 'apt-get install -y libltdl-dev'
                sh 'docker run hello-world'
            }
        }
    }
}
