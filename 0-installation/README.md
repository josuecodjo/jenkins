# Installation

- Sur serveur Ubuntu 22.04

```
multipass launch -n jenkins -c 2 -m 4G -d 40G jammy
Multipass shell jenkins
Sudo apt update
Sudo apt install openjdk-11-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add –
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list’
sudo apt update
sudo apt install Jenkins
sudo systemctl start Jenkins
sudo systemctl enable Jenkins
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
  - openjdk-11-jdk

write_files:
  - content: |
      #!/bin/bash

      wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
      sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
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
Multipass shell jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

```
- Avec Docker

```
Docker run –d –name jenkins –p 8888:8080 –v $PWD/var/jenkins_home jenkins/jenkins:lts-jdk11
Docker exec –it jenkins cat /var/lib/jenkins/secrets/initialAdminPassword
```