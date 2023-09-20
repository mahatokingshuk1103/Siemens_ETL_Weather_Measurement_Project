pipeline 
{
    agent any

environment {
        imageName = "kingshuk0311/siemens"
        imageTag = "v${env.BUILD_ID}"
        dockerfile = "./Dockerfile"
        SSH_CREDENTIALS = credentials('kopssiemensid')  // Replace with your SSH credential ID
        KOPS_CLUSTER_NAME = 'kingshuk.shop'
        KOPS_INSTANCE_IP = 'ip-172.31.32.55' 
    }


options {
        skipStagesAfterUnstable()
    }

    
    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'main', credentialsId: 'gitid', url: 'https://github.com/mahatokingshuk1103/Siemens_ETL_Weather_Measurement_Project.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                  sh "sudo -S docker build -t ${imageName}:${imageTag} -f ${dockerfile} ."
                  echo "${imageName}:${imageTag}"
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                script {
                   docker.withRegistry('https://us-east-2.console.aws.amazon.com/ecr/repositories/public/722525113337/weather?region=us-east-2', 'ecr:us-east-2:AWS Credentials id') {
                   app.push("${env.BUILD_NUMBER}")
		   app.push("latest")
                }
            }
        }

       
    }
}
}
