pipeline 
{
    agent any

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
                   app = docker.build("weather")
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
