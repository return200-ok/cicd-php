#Install Symfony on Ubuntu 20.04
```
sudo apt update
sudo apt upgrade
sudo apt install php php-json php-ctype php-curl php-mbstring php-xml php-zip php-tokenizer php-tokenizer libpcre3 --no-install-recommends
sudo apt install git zip unzip
```
##After that, download and install the symfony-cli tool
```
wget https://get.symfony.com/cli/installer -O - | bash
```
##After that, add the Symfony installation path to the PATH user.
```
export PATH="$HOME/.symfony/bin:$PATH".
mv /home/$USER/.symfony5/bin/symfony /usr/local/bin/symfony
```
##And apply the changes by running
```
source ~/.bashrc
```