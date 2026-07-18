# Project4

#STEP 1Install Jenkins
Install path:  https://www.jenkins.io/doc/book/installing/linux/


sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version

#STEP 2 Jenkins Long term
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins


sudo systemctl enable jenkins

sudo systemctl start jenkins

Get Admin Password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

#STEP 3 Install Docker..
sudo apt install docker.io -y

sudo systemctl enable docker

sudo systemctl start docker

#STEP 4  Give jenkins access to docker
sudo usermod -aG docker jenkins

****
git init

git add .

git commit -m "Initial Commit"

git branch -M main

git remote add origin YOUR_REPOSITORY_URL

git push -u origin main
****
#Install following plugins in jenkins
Install:

Pipeline
Git
Git Client
Docker Pipeline
Credentials Binding
GitHub


<br>
Add credentials in jenkins
Manage Jenkins
  -> Credentials
  -> System
  -> Global Credentials

  Username : Docker Hub Username
Password : Docker Hub Password / Access Token
ID       : dockerhub

***
jenkinfile

pipeline {

    agent any

    environment {
        IMAGE_NAME = "YOUR_DOCKERHUB_USERNAME/nodejs-cicd"
        CONTAINER_NAME = "nodejs-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'YOUR_GITHUB_REPOSITORY_URL'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push ${IMAGE_NAME}:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true

                docker pull ${IMAGE_NAME}:latest

                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 3000:3000 \
                ${IMAGE_NAME}:latest
                '''
            }
        }
    }
}

groups jenkins

#STEP 5 install git


