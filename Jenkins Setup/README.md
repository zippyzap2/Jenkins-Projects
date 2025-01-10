# Introduction
Installing and running jenkins in Ubuntu virtual machine

## Create  a Virtual Machine in AWS

<img width="500" alt="Screenshot 2025-01-10 at 1 27 47 PM" src="https://github.com/user-attachments/assets/12b1dab9-9aaf-4be6-9967-84dd0d72b8ff" />

- Establish SSH connection

## Install jenkins 
- https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```
<img width="500" alt="Screenshot 2025-01-10 at 1 31 47 PM" src="https://github.com/user-attachments/assets/9323da35-faf8-4123-b2a2-8ef21a926725" />

- check the status
  ```
  sudo systemctl status jenkins
  ```
- if status is failed, then check the log
  ```
  journalctl -u jenkins
  ```
- if you see this error "jenkins: failed to find a valid Java installation" check java installed or not if not you need to install java
  ```
  # check
  java -version

   # install from here https://www.jenkins.io/doc/book/installing/linux/#installation-of-java
   sudo apt update
   sudo apt install fontconfig openjdk-17-jre -y

  ```
<img width="500" alt="Screenshot 2025-01-10 at 1 55 07 PM" src="https://github.com/user-attachments/assets/3a997e93-0b22-408c-86f2-d0fb9ec01247" />

## Restart jenkins service
```
sudo systemctl restart jenkins
```
- check status now
  
<img width="500" alt="Screenshot 2025-01-10 at 2 02 56 PM" src="https://github.com/user-attachments/assets/d1a06681-647c-47e9-96b8-b817317aac25" />

## Access jenkins
- Try to access jenkins using public ip of VM at port 8080
- example - 13.57.211.144:8080

<img width="500" alt="Screenshot 2025-01-10 at 2 27 48 PM" src="https://github.com/user-attachments/assets/ac81577d-a9c0-4b6c-af09-8bf29e9075a0" />

- for password check journalctl and find there
  ```
  journalctl -u jenkins

  # you can do this also
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```
<img width="500" alt="Screenshot 2025-01-10 at 2 28 58 PM" src="https://github.com/user-attachments/assets/adfb8641-5fa1-4658-adca-09ca53a27490" />

## Install required plugins

<img width="500" alt="Screenshot 2025-01-10 at 2 29 53 PM" src="https://github.com/user-attachments/assets/335dd7c3-5638-4715-b37f-55d96925f6c4" />

- Start using jenkins

<img width="500" alt="Screenshot 2025-01-10 at 2 43 00 PM" src="https://github.com/user-attachments/assets/9cf01156-dba5-48a5-91a8-124f94b9c9aa" />



