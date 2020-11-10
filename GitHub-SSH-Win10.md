## Connecting to GitHub using SSH protocol
This page explains how to create and use ssh keys in order to access your github repositories.
### Creation of OpenSSH key pair
Keys creation is done on OpenBSD using `ssh-keygen` and the `.pub` key is transfered to your GitHub account. There are several steps to be taken:
* Create key pair
* Transfer public key to GitHub
* Store keys in Windows and setup of ssh-agent service (if not running)

### Create `.ssh` directory in Windows 10
Create `.ssh` directory under `C:\Users\D%USERNAME%\.ssh` and store you keys in there
### Paste you `.pub` key in your GitHub account
Log into your GitHub account, then go to Profile-->Settings-->SSH and GPG keys ([Link](https://github.com/settings/keys "https://github.com/settings/keys")). Click on New SSH Key. Name the key and paste the contect of .pub key. Then, click on Add SSH key. All done.
### Checking and starting the `ssh-agent` service
Open PowerShell terminal by pressing WindowsKey + S to open searchbox and type in: powershell. Right click non it to make sure you're running as an Administrator. Then type:
```
Set-Service ssh-agent -StartupType Manual
Start-Service ssh-agent
Get-Service ssh-agent
```
We set the service to manual (start only when needed), start the service and check if it is running.
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