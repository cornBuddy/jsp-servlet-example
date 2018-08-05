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
                          -v /var/run/docker.sock:/var/run/docker.sock \
                          -v /usr/lib/x86_64-linux-gnu/libltdl.so.7:/usr/lib/x86_64-linux-gnu/libltdl.so.7'
                }
            }
            steps {
                sh 'docker run hello-world'
            }
        }
    }
}
