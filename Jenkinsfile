pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                 app = docker.build("skeppy")
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        stage('Push') {
            steps {
                script{
                        docker.withRegistry('https://298050056371.dkr.ecr.ap-south-1.amazonaws.com/skeppy', 'ecr:ap-south-1:aws-credential') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
	stage('Deploy'){
            steps {
		 sh 'chmod +x -R /home/ubuntu/bin/kubectl'
                 sh '/home/ubuntu/bin/kubectl apply -f deployment.yml'
            }
        }
    }
}
