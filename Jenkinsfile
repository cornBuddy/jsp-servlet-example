#!groovy

node {
    stage('Pull from SCM') {
        git 'https://github.com/cornBuddy/jsp-servlet-example/'
        sh 'ls -la ./'
        sh 'cat ./settings.xml'
        sh "ls -la ${env.WORKSPACE}"
    }

    docker.image('maven:3-alpine')
        .inside("--env SONAR_URL=${env.SONAR_URL} -v ${env.WORKSPACE}/settings.xml:/root/.m2/settings.xml --user root") {
            stage('Code analysis') {
                sh 'cat /root/.m2/settings.xml'
                sh 'echo $SONAR_URL'
                sh 'ping -c 4 $SONAR_URL'
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
