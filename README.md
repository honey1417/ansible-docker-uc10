# ansible-docker
### Your Jenkins user has /bin/false as its shell, which prevents it from 
### logging in interactively or using SSH keys properly. You need 
### to change the shell to /bin/bash so Jenkins can use SSH.
```
sudo usermod -s /bin/bash jenkins
cat /etc/passwd | grep jenkins
```
