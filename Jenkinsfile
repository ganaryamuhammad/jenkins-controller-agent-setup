groovy
pipeline {
    agent { label 'linux' }

    stages {
        stage('Info') {
            steps {
                sh 'whoami'
                sh 'hostname'
                sh 'pwd'
                sh 'java -version'
            }
        }
    }
}
