# ansible-docker
### Your Jenkins user has /bin/false as its shell, which prevents it from logging in interactively or using SSH keys properly. You need to change the shell to /bin/bash so Jenkins can use SSH.
```
sudo usermod -s /bin/bash jenkins
cat /etc/passwd | grep jenkins
```

Solution: Create the jenkins User on theHost (Without Installing Jenkins)

Log in to the Host as Root
```
ssh root@host_ip
```
Create the jenkins User on the Host
```
sudo useradd -m -s /bin/bash jenkins
sudo passwd -d jenkins  # Remove password (so only SSH keys are used)
```
Allow Jenkins User to Use Sudo (If Needed)
```
echo "jenkins ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/jenkins

sudo chmod 440 /etc/sudoers.d/jenkins
```
Set Up SSH Access for Jenkins User
```
sudo chmod 700 /home/jenkins/.ssh

sudo chown -R jenkins:jenkins /home/jenkins/.ssh

echo "ssh-rsa AAAA...your-public-key..." | sudo tee -a /home/jenkins/.ssh/authorized_keys
```

Set correct permissions:
```
sudo chmod 600 /home/jenkins/.ssh/authorized_keys
sudo chown -R jenkins:jenkins /home/jenkins/.ssh
sudo nano /etc/ssh/sshd_config
```

Ensure these lines exist and are not commented out
```
PubkeyAuthentication yes
PasswordAuthentication no
AllowUsers jenkins root

sudo systemctl restart sshd
ssh -i ~/.ssh/id_rsa jenkins@host_ip
```
