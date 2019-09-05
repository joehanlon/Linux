https://linuxize.com/post/how-to-install-visual-studio-code-on-ubuntu-18-04/  
#### Install the dependencies  
sudo apt update  
sudo apt install software-properties-common apt-transport-https wget  
#### Import the Microsoft GPG key 
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -  
#### Enable VSCode repo 
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"  
#### Install the latest version 
sudo apt update  
sudo apt install code  

#### Start VSCode
code _(In the terminal)_  
code . _(start in a particular directory)_

#### Update VSCode
sudo apt update  
sudo apt upgrade  

Pull up extensions : ctrl+shift+x  
Pull up command palette : ctrl+shift+p  

https://scotch.io/bar-talk/22-best-visual-studio-code-extensions-for-web-development  
