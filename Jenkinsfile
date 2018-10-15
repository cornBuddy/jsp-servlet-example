#!groovy

node {
    stage('Pull from SCM') {
        git 'https://github.com/cornBuddy/jsp-servlet-example/'
    }

    docker.image('maven:3-alpine')
        .inside("--env SONAR_URL=${env.SONAR_URL} -v ${env.WORKSPACE}/m2:/root/.m2 --user root") {
            stage('Code analysis') {
                sh 'ping -c 4 http://sonarqube'
                sh 'cat /root/.m2/settings.xml'
                sh 'mvn clean verify sonar:sonar'
            }

            stage('Build') {
                sh 'mvn clean package'
            }
        }

    stage('Deploy') {
        isBuildSucceeded = currentBuild.result == null \
            || currentBuild.result == 'SUCCESS'
        if (isBuildSucceeded) {
            sh 'rm -rf deploy-to-tomcat-cluster || true'
            sh 'git clone https://github.com/cornBuddy/deploy-to-tomcat-cluster'
            sh 'cp target/jsp-servlet-example.war deploy-to-tomcat-cluster'
            dir('./deploy-to-tomcat-cluster') {
                ansiblePlaybook(
                    inventory: 'inventory',
                    playbook: 'rolling-update.yml',
                    extraVars: [
                        deploy_uri: 'jsp-servlet-example',
                        artifact_path: './jsp-servlet-example.war',
                        deployer: "${env.DEPLOYER}",
                        deployer_password: "${env.DEPLOYER_PASSWORD}",
                    ],
                )
            }
        }
    }
}
