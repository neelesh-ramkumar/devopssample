Installing Docker...
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker --now
docker
![Screenshot from 2025-03-20 11-45-57](https://github.com/user-attachments/assets/1933ac46-61e9-4b39-b451-5aa1930f482a)
![Screenshot from 2025-03-20 11-46-11](https://github.com/user-attachments/assets/70d62704-d157-4152-b336-8da8836a60b2)
Download Docker plugins in Jenkins

    Go to Jenkins Dashboard -> Manage Jenkins -> Available Plugins -> Search Docker
    Select these plugins and Install
        Docker
        Docker Commons
        Docker Pipeline
        docker-build-step
        CoudBees Docker Build and Publish
![image](https://github.com/user-attachments/assets/340f7b8d-f757-4012-8fc4-40c4d5675b67)
![image](https://github.com/user-attachments/assets/2afbdb4d-2101-46de-ba5b-ee974daea638)
![Screenshot from 2025-03-20 10-34-59](https://github.com/user-attachments/assets/e548de54-b984-4822-85e4-f95b909c10e9)
![Screenshot from 2025-03-20 10-39-34](https://github.com/user-attachments/assets/362c4d42-d757-441d-9593-71a27d776875)
![Screenshot from 2025-03-20 10-37-00](https://github.com/user-attachments/assets/17ffe6b1-11e3-41fb-bdf3-15601b0761ac)
Creating and building a pipeline..
Go to Jenkins Dashboard > Create a Job

    Enter a project name
    Select pipeline
    Click Ok

    Go to pipeline
    Paste this script below and change the credential wherever mentioned:

pipeline {
    agent any

    environment {
        IMAGE_NAME = "sanjai4334/docker"          // Replace with your Docker Hub username and image name
        TAG = "latest"
        CONTAINER_NAME = "my-container"
        PORT = "3001"
    }

    stages {
        
        stage('Clone Repository') {
            steps {
                echo "Cloning GitHub repository..."
                git 'https://github.com/sanjai4334/docker.git'  // Replace with your repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

                stage('Login to Docker Hub') {
            steps {
                echo "Logging into Docker Hub..." // Change the cedentialsID if you have docker credentials already added with another id other than docker-seccred
                withCredentials([usernamePassword(credentialsId: 'docker-seccred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh "docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:$TAG"
                sh "docker push $IMAGE_NAME:$TAG"
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo "Deploying Docker container..."
                sh 'chmod +x deploy.sh'
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
lick save
click build
check the pipeline construction by clicking build now ..
![Screenshot from 2025-03-20 11-16-20](https://github.com/user-attachments/assets/87a5d046-3f3c-4b53-b3f7-0c306c31e4db)
![Screenshot from 2025-03-21 11-19-17](https://github.com/user-attachments/assets/546f60e7-b8e3-445a-8e2a-1ff9e5f69f9c)
Go to dockerhub and see your image is pushed or not : 
![Screenshot from 2025-03-20 11-16-49](https://github.com/user-attachments/assets/bf1014fb-7b38-4fec-96f8-06dc141efe1d)

