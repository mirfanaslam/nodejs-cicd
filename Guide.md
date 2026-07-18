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

groups jenkins

#STEP 5 install git


