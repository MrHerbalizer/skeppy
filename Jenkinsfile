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
		 sh 'curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.4/2023-08-16/bin/linux/amd64/kubectl'
		 sh 'chmod u+x ./kubectl'
                 sh 'kubectl apply -f deployment.yml'
            }
        }
    }
}
