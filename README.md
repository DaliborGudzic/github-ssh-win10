## Connecting to GitHub using SSH protocol
This page explains how to create and use ssh keys in order to access your github repositories. Keys creation is done on OpenBSD and the `.pub` key is transfered to GitHub. There are several steps to be taken:
---
### Create `.ssh` directory
Create `.ssh` directory under `C:\Users\D%USERNAME%\.ssh` and store you keys in there
## Paste you `.pub` key in your GitHub account
Log into your GitHub account, then go to Profile-->Settings-->SSH and GPG keys ([Link][test](https://github.com/settings/keys)). Click on New SSH Key. Name the key and paste the contect of .pub key. Then, clock on Add SSH key. All done.
### Checking and starting ssh-agent service
Open PowerShell terminal by pressing WindowsKey + S to open searchbox and type in: powershell. Right click non it to make sure you're running as an Administrator. Then type:
```
Set-Service ssh-agent -StartupType Manual
Start-Service ssh-agent
Get-Service ssh-agent
```
We set the service to manual (start only when needed); start the service and check if it is running.
## Adding the key to ssh-add
Add a ssh key
```
ssh-add C:\Users\D%USERNAME%\.ssh\your.ed25519.key
```
Show keys managed by the ssh-agent
```
ssh-add -l
```
For git, add a system environment variable:
```
$env:GIT_SSH="C:\Windows\System32\OpenSSH\ssh.exe"
```