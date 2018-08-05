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
            agent any
            when {
                expression {
                    currentBuild.result == null \
                        || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                sh 'rm -rf deploy-to-tomcat-cluster || true'
                sh 'git clone https://github.com/cornBuddy/deploy-to-tomcat-cluster'
                sh 'cp target/jsp-servlet-example.war deploy-to-tomcat-cluster'
                sh 'cd deploy-to-tomcat-cluster'
                ansiblePlaybook(
                    inventory: 'deploy-tomcat-cluster/inventory',
                    playbook: 'deploy-tomcat-cluster/playbook.yml',
                    extraVars: [
                        deploy_uri: 'jsp-servlet-example',
                        artifact_path: 'deploy-to-tomcat-cluster/jsp-servlet-example.war',
                    ],
                )
            }
        }
    }
}
