# GitLab-installation
Step 1 - Update Repository and Upgrade Packages
sudo apt update
sudo apt upgrade -y

Step 2 - Install Gitlab Dependencies
sudo apt install curl openssh-server ca-certificates postfix -y

Step 3 - Install GitLab
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo apt install gitlab-ce -y
sudo vim /etc/gitlab/gitlab.rb
 external_url 'http://git.acirrustech.com'

Step 4 - Generate Let's encrypt SSL Certificate and DHPARAM Certificate
sudo apt install letsencrypt -y
sudo letsencrypt certonly --standalone --agree-tos --no-eff-email --agree-tos --email addiani744@gmail.com -d gitacirrustech.clear27.com

sudo mkdir -p /etc/gitlab/ssl
sudo openssl dhparam -out /etc/gitlab/ssl/dhparams.pem 2048
chmod 600 /etc/gitlab/ssl/*

Step 5 - Configure HTTPS for GitLab
sudo vim /etc/gitlab/gitlab.rb
paste under GitLab Nginx:
nginx['redirect_http_to_https'] = true
nginx['ssl_certificate'] = "/etc/letsencrypt/live/gitacirrustech.clear27.com/fullchain.pem"
nginx['ssl_certificate_key'] = "/etc/letsencrypt/live/gitacirrustech.clear27.com/privkey.pem"
nginx['ssl_dhparam'] = "/etc/gitlab/ssl/dhparams.pem"
sudo gitlab-ctl reconfigure

Step 6 - Configure Ubuntu UFW Firewall
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw enable
sudo ufw status

Step 7 - GitLab Post-Installation:
type the URL in your browser"gitacirrustech.clear27.com"
