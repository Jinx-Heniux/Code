# Quickstart

## Install Go

```shell
# Install Go on Ubuntu 20.04
# https://go.dev/doc/install
# https://www.digitalocean.com/community/tutorials/how-to-install-go-on-ubuntu-20-04


#################### Step 1 — Installing Go ####################

# Download Go: https://go.dev/dl/
curl -OL https://go.dev/dl/go1.18.1.linux-amd64.tar.gz
# 或
wget https://golang.google.cn/dl/go1.16.6.linux-amd64.tar.gz

# To verify the integrity of the file you downloaded, 
# run the sha256sum command and pass it to the filename as an argument:
sha256sum go1.18.1.linux-amd64.tar.gz

# Remove any previous Go installation by deleting the /usr/local/go folder 
# (if it exists), then extract the archive you just downloaded into /usr/local, 
# creating a fresh Go tree in /usr/local/go:
# rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.1.linux-amd64.tar.gz
rm -rf /usr/local/go
sudo tar -C /usr/local -xvf go1.18.1.linux-amd64.tar.gz
# 或
sudo tar -zxvf go1.16.6.linux-amd64.tar.gz -C /usr/local



#################### Step 2 — Setting Go Paths ####################

# Use your preferred editor to open .profile, 
# which is stored in your user’s home directory. 
# Here, we’ll use nano:
sudo nano ~/.profile
# 或
suoo nano $HOME/.bash_profile

# Then, add the following information to the end of your file:
export PATH=$PATH:/usr/local/go/bin
# 或
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
# 或
export GOROOT="/usr/local/go"
export GOPATH=$HOME/go
export GOBIN=$GOROOT/bin
export PATH=$PATH:$GOBIN

# Next, refresh your profile by running the following command:
source ~/.profile
# 或
source $HOME/.bash_profile

# After, check if you can execute go commands by running go version:
go version



#################### Step 3 — Testing Your Install ####################

# By default, the GOPATH variable, 
# which specifies the location of the workspace is set to $HOME/go. 
# To create the workspace directory type:
mkdir ~/go

# Inside the workspace create a new directory src/hello:
mkdir -p ~/go/src/example.jinx.com/hello # domain_name/project_name/

# When importing packages, you have to manage the dependencies 
# through the code’s own module. 
# You can do this by creating a go.mod file with the go mod init command:
cd ~/go/src/example.jinx.com/hello # domain_name/project_name/
go mod init example.jinx.com/hello

# and in that directory create a file named hello.go:
vim hello.go

# Test your code to check that it prints the Hello, World! greeting:
go run .


#################### Step 4 — Turning Your Go Code Into a Binary Executable ####################

# Navigate** to the ~/go/src/hello directory and 
# run go build to build the program:
cd ~/go/src/example.jinx.com/hello
go build
# The command above will build an executable file named hello.

# You can run the executable by simply executing the command below:
./hello
```

