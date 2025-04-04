pipeline {
    tools {
        nodejs 'nodejs-23.11.0'
        git 'Default'
    }
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy to staging'
                sh 'echo "Hello" >> rez.txt'
            }
        }
    }
}

