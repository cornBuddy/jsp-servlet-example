#!groovy

node('maven') {
    stage('Pull from SCM') {
        git 'https://github.com/cornBuddy/jsp-servlet-example/'
    }

    stage('Build') {
        sh 'mvn clean package'
    }

    stage('Deploy') {
        echo 'deploying...'
    }
}
