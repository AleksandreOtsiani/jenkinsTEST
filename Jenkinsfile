pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                dir('cicd-pipeline'){
                    sh 'npm install'
                }
            }
        }

        stage('Test') {
            steps {
                dir('cicd-pipeline'){
                    sh 'npm test'
                }    
            }
        }
        stage('Docker build') {
            steps {
                dir('cicd-pipeline'){
                    script {
                        if (env.BRANCH_NAME == 'main') {
                            sh 'docker build -t nodemain:v1.0 .'
                        } else {
                            sh 'docker build -t nodedev:v1.0 .'
                        }
                    }    
                }
            }
        }
        stage('Deploy') {
            steps {
                dir('cicd-pipeline'){
                    script {
                        if (sh (script: "docker ps -q | grep .", returnStatus: true) == 0) {
                            sh "docker stop \$(docker ps -q)"
                            sh "docker rm \$(docker ps -a -q)"
                        } 
                        else{
                            echo "No running containers found."
                        }
                        if (env.BRANCH_NAME == 'main') {
                            sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0'
                        } else {
                            sh 'docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0'
                        }   
                    }
                }
            }
        }
    }
}