pipeline {
    agent any

    // Define variables for Docker image and tag
    environment {
        imageName = "kingshuk0311/siemens"
        imageTag = "v${env.BUILD_ID}"
        dockerfile = "./Dockerfile"
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
                    // Build the Docker image using the defined variables
                    sh "sudo -S docker build -t ${imageName}:${imageTag} -f ${dockerfile} ."
                    echo "${imageName}:${imageTag}"
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub using credentials
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubpwd')]) {
                        sh "sudo docker login -u kingshuk0311 -p \${dockerhubpwd}"
                    }

                    // Push the Docker image to Docker Hub
                    sh "sudo docker push ${imageName}:${imageTag}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            agent { label 'dev' }
            steps {
                script {
                    // Define Helm command to upgrade or install the chart
                    def helmCmd = "helm upgrade --install --namespace=test3 foptgwetherking-stack helm/wprofilecharts --set appimage=${imageName}:${imageTag}"
                    
                    // Execute the Helm command
                    sh(helmCmd)
                }
            }
        }
    }
}
