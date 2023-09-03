# Installation

- Sur serveur Ubuntu 22.04

```
multipass launch -n jenkins -c 2 -m 4G -d 40G jammy
multipass shell jenkins
sudo apt update
sudo apt install openjdk-17-jre -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo ufw allow 8080
sudo cat /var/lib/jenkins/secrets/initialAdminPassword (Récupérer le mot de passe initial)
```

- Sur Serveur Cloud avec cloud-init

```
### init-jenkins.yml  
groups:
  - admin

users:
  - default
  - name: josuecodjo # --> username
    gecos: Josue A. Codjo #--> fullname
    groups: sudo, admin
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
package_update: true
package_upgrade: true
packages:
  - python3
  - python3-pip
  - python3-venv
  - net-tools
  - openjdk-17-jre

write_files:
  - content: |
      #!/bin/bash

      curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
      echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
      sudo apt update
      sudo apt install jenkins
      sudo systemctl start jenkins
      sudo systemctl enable jenkins
      sudo ufw allow 8080
    path: jenkinsapp.sh
    permissions: '0755'
    append: true

runcmd:
  - 'sudo -u ubuntu bash jenkinsapp.sh'

##### command

multipass launch -n jenkins -c 2 -m 4G -d 40G --cloud-init init-jenkins.yml  jammy
multipass shell jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

```
- Avec Docker

```
docker run -d --name jenkins -p 8888:8080 -v jenkins-vol1:/var/jenkins_home jenkins/jenkins:lts-jdk11
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```