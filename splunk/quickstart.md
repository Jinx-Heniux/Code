# Quickstart



```shell
# Install Splunk on Ubuntu 20.04 
# How to Install Splunk Data platfrom on Ubuntu 20.04 Linux - Linux Shout
# https://www.how2shout.com/linux/how-to-install-splunk-data-platfrom-on-ubuntu-20-04-linux/

# Download splunk https://www.splunk.com/en_us/download/splunk-enterprise.html
wget -O splunk-8.2.5-77015bc7a462-linux-2.6-amd64.deb "https://download.splunk.com/products/splunk/releases/8.2.5/linux/splunk-8.2.5-77015bc7a462-linux-2.6-amd64.deb"

# Install Splunk
sudo apt install ./splunk-*-amd64.deb

# Accept License, Enable Boot start and Set Admin user & password
sudo /opt/splunk/bin/splunk enable boot-start

# Access Spunk Web interface
http://localhost:8000

# Login Admin account
username: admin

# Uninstall Splunk Enterprise (optional)
sudo /opt/splunk/bin/splunk disable boot-start
sudo apt remove splunk
```



