# Quickstart



```shell
# Install Anaconda on Ubuntu 20.04
# https://linuxize.com/post/how-to-install-anaconda-on-ubuntu-20-04/
# https://docs.anaconda.com/anaconda/install/linux/

# Anaconda Navigator is a QT-based GUI. If you are installing Anaconda on a desktop machine and you want to use the GUI application, install the following packages. Otherwise, skip this step.
sudo apt install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6

# Download the Anaconda installation script with your web browser or wget :
wget -P /tmp https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh

# This step is optional, but it is recommended to verify the data integrity of the script.
# Use the sha256sum command to display the script checksum:
sha256sum /tmp/Anaconda3-2020.02-Linux-x86_64.sh

# Run the script to start the installation process:
bash /tmp/Anaconda3-2020.02-Linux-x86_64.sh

# To activate the Anaconda installation, you can either close and re-open your shell or load the new PATH environment variable into the current shell session by typing:
source ~/.bashrc

# To verify the installation type conda in your terminal.
cd ~/anaconda3/bin
./conda

# If you installed Anaconda on a Desktop system, open the Navigator GUI by entering anaconda-navigator in your terminal:
cd ~/anaconda3/bin
./anaconda-navigator
```

