/* groovylint-disable-next-line CompileStatic */
pipeline {

    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-p 3000:3000'
        }
    }
    tools {
        nodejs 'nodejs-23.11.0'
        git 'Default'
    }
    environment {
        CI = 'true'
        APP_NAME = 'lab4-application'
        APP_VERSION = '1.0.0'
        FILE_NAME_1 = 'output/info_build.txt'
    }
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
            agent any
            steps {
                echo 'Deploy to staging'
                sh 'mkdir -p output'
                sh 'echo "Hello" >> output/rez.txt'
                sh 'env >> output/info_env.txt'
                sh 'set >> output/info_set.txt'
                writeFile file: 'output/usefulFile.txt', text: 'This is useful, need to archive it.'
                sh 'echo "BUILD_ID: $BUILD_ID" > $FILE_NAME_1'
                sh 'echo "BUILD_TAG: $BUILD_TAG" >> $FILE_NAME_1'
                sh 'docker --version'
            }
        }
        stage('Artifacts') {
            steps {
                // Archive the build output artifacts.
                archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.log'
            }
        }
    }
}

