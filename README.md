# $${\color{red} \textbf{Project : Netflix}}$$

## ${\color{red} \textbf{Phase 1: Initial Setup and Deployment}}$

${\color{green} \textbf{1. Generate Token From TMDB}}$

$\color{green} \textbf{2. Launch ec2 instance}$
- Ubuntu
- t2.medium
- storage : 40
- SG : All traffic

$\color{green} \textbf{3. Git Clone }
````
git clone <repo>
````
$\color{green} \textbf{4. Install Docker and set up}$
````
sudo apt update 
sudo apt install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
````
$\color{green} \textbf{5. Build the image with TBDM API and run the container}$
````
docker build --build-arg TMDB-V3-API-KEY=<your-api-key> -t netflix .
docker run -d --name netflix -p 8081:80 netflix:latest
````
${\color{green} \textbf{6. Host : pub-ip:8081}}$

![image](https://github.com/user-attachments/assets/5930247a-559a-4149-9f0f-effa41be5f5c)



## ${\color{red} \textbf{Phase 2: Security}}$

### $\color{purple} \textbf{Install SonarQube and Trivy for Scan }$

$\color{green} \textbf{1. SonarQube for Code Testing with Direct pulling Image}$

````
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
````
$\color{green} \textbf{ 2. To Access SonarQube : pub-ip:9000}$

$\color{red} \textbf{ NOTE : }$ SonarQube have by default username and password is admin.


$\color{green} \textbf{ 3. Generate Token in SonarQube; it will need us in jenkins }$
- My Account  → Security → Token name  → Generate Token → Copy that Token


$\color{green} \textbf{ 4. Install Trivy for Scan Image}$

````
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
````
$\color{green} \textbf{5. Scan Netflix Image}$
````
trivy image <imageid>
````

## ${\color{red} \textbf{Phase 3: CI/CD Setup}}$

$\color{green} \textbf{1. Install Jenkins for Automate Deployment}$
````
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
````
$\color{green} \textbf{2. To Access Jenkins : pub-ip:8080}$
- Add initial password
- Create user with password
- Start using jenkins

$\color{green} \textbf{3. Install Necessary Plugins}$

Goto Manage Jenkins → Plugins → Available Plugins → Install below plugins

1. Eclipse Temurin Installer 
2. SonarQube Scanner 
3. NodeJs 
4. Email Extension 

$\color{green} \textbf{4. Set up Plugins in Tool}$

Goto Manage Jenkins → Tool → 
- Add Node name : node16 / version : 16.16.0
- Add java name : jdk17 / version : 17.0.1+12 (automatic installation)
- Add SonarQube Scanner Name : sonar-scanner 

