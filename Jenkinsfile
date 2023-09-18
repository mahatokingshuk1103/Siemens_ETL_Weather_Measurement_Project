pipeline {
    agent any

imageName = "kingshuk0311/vivek5"
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
                   sh "sudo -S docker build -t ${imageName}:${imageTag} ."
                    echo "${imageName}:${imageTag}"
                }
            }
        }

        stage('Push Image Dockerhub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubpwd')]) {
                        sh "sudo docker login -u kingshuk0311 -p \${dockerhubpwd}"
                    }
                    sh "sudo docker push ${imageName}:${imageTag}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            agent { label 'dev' }
            steps {
                script {
                  def helmCmd = "helm upgrade --install --namespace=test3  foptgwetherking-stack helm/wprofilecharts --set appimage=kingshuk0311/vivek5:v11980"
                  

                  sh(helmCmd)

                }
            }
        }
    }
}
