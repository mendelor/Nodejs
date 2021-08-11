pipeline {
    agent any
    environment {
       branch = 'master'
     }
    stages {
        stage('Cloning Git') {
          options {
                timeout(time: 2, unit: "MINUTES")
            }
            steps {
                git (
                   url: 'git@github.com:mendelor/nodejs',
                   credentialsId: 'node_example',
                   branch: "${branch}"
                )
            }
        }
        stage('Clean_Older_builds') {
            steps {
                echo 'Starting the Pipeline'
                sh 'docker ps -f name=httpd -q | xargs --no-run-if-empty docker container stop'
                sh 'docker container ls -a -fname=httpd -q | xargs -r docker container rm'
            }
        }
        stage('Build_Image') {
            steps {
                script {
                    myapp = docker.build("mendelor/nodejs")
                }
            }
        }
        stage('Test_Image') {
            steps {

                echo 'test'
            }
        }
        stage('Approval') {
            input {
               message 'Should we continue?'
             }
            steps {
                echo 'Approval'
            }
        }
        stage('Run_Image') {
            steps {
                script {
                    myapp.run("-p 3001:3000 mendelor/nodejs")
                }
            }
        }
    }
}
