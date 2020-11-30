## Connecting to GitHub using SSH protocol
This page explains how to create and use ssh keys in order to access your github repositories.
### Creation of OpenSSH key pair
Keys creation is done on OpenBSD using `ssh-keygen` and the `.pub` key is transfered to your GitHub account. There are several steps to be taken:
* Create key pair
* Transfer public key to GitHub
* Store keys in Windows and setup of ssh-agent service (if not running)
```
d@gate:~$ ssh-keygen -t ed25519 -C "GitHub key" -f /home/d/.ssh/github.key
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /tmp/github.key
Your public key has been saved in /tmp/github.key.pub
The key fingerprint is:
SHA256:mscabM4K5bv1NeMVx3/99AhpTlH5BNbXU2WYjWkxJ9c GitHub key
The key's randomart image is:
+--[ED25519 256]--+
|              *O@|
|             .*XE|
|             .o +|
|             o o |
|    .   S   o o .|
|   o . +     = ..|
|  . . B o + *   =|
|   . * = o B . o+|
|    +o+ . . . . o|
+----[SHA256]-----+
d@gate:~$
```
### Create `.ssh` directory in Windows 10
Create `.ssh` directory under `C:\Users\D%USERNAME%\.ssh` and store you keys in there
### Paste you `.pub` key in your GitHub account
Log into your GitHub account, then go to Profile-->Settings-->SSH and GPG keys ([https://github.com/settings/keys](https://github.com/settings/keys)). Click on New SSH Key. Name the key and paste the contect of .pub key. Then, click on Add SSH key. All done.
### Checking and starting the `ssh-agent` service
Open PowerShell terminal by pressing WindowsKey + S to open searchbox and type in: powershell. Right click non it to make sure you're running as an Administrator. Then type:
```
Set-Service ssh-agent -StartupType Manual
Start-Service ssh-agent
```
We set the service to manual (start only when needed) and start the service.
```
PS C:\Users\D> Get-Service ssh-agent

Status   Name               DisplayName
------   ----               -----------
Running  ssh-agent          OpenSSH Authentication Agent


PS C:\Users\D>
```
We verify the service is running.
### Adding the key to ssh-add
We add the key to ssh-add so it is active and stored in memory. In case we need to use the key, there is no need for repeated loading, password tyiping etc.

Add your ssh key (in this case an example private key)
```
ssh-add C:\Users\D%USERNAME%\.ssh\your.github.ed25519.key
```
Show keys managed by the ssh-agent
```
ssh-add -l

PS C:\Users\D> ssh-add -l
256 SHA256:lHALxP0lAmO2xaudBxz5XWx76jy4ORkEzCS9v5ePwu4 GitHub-ed25519-key (ED25519)
PS C:\Users\D>
```
For git, add a system environment variable:
```
$env:GIT_SSH="C:\Windows\System32\OpenSSH\ssh.exe"
```
### Profit
Finally, we can use git (over a terminal in VSC actually) over SSH. First we test the connection:
```
ssh -T git@github.com

PS C:\Users\D\> ssh -T git@github.com
Hi DaliborGudzic! You've successfully authenticated, but GitHub does not provide shell access.
PS C:\Users\D> 
```
If it is successful, we managed to do everything right. If not go cry for half an hour, then wipe the tears, take a shot of rakia and re-read the steps.

Added on 30Nov2020:
Lets make some changes:
```
PS C:\xampp\htdocs\projects\itbootcamp\githubProjects\github-ssh-win10> git add .\github-ssh-win10.md 
PS C:\xampp\htdocs\projects\itbootcamp\githubProjects\github-ssh-win10> git commit -m "Added output of pushing changes"
[master d70d39b] Added output of pushing changes
 1 file changed, 20 insertions(+), 1 deletion(-)
PS C:\xampp\htdocs\projects\itbootcamp\githubProjects\github-ssh-win10> git push origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 762 bytes | 381.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0        
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:DaliborGudzic/github-ssh-win10.git
   2a5dff3..d70d39b  master -> master
```