# Quickstart

```bash
# Ubuntu 20.04
# Step 1 — Installing MongoDB

# To start, import the public GPG key for the latest stable version of MongoDB by running the following command.
curl -fsSL https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

# If you’d like to double check that the key was added correctly, you can do so with the following command:
apt-key list

# Run the following command, which creates a file in the sources.list.d directory named mongodb-org-4.4.list.
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

# After running this command, update your server’s local package index so APT knows where to find the mongodb-org package:
sudo apt update

# Following that, you can install MongoDB:
sudo apt install mongodb-org

# Step 2 — Starting the MongoDB Service and Testing the Database

# Run the following systemctl command to start the MongoDB service:
sudo systemctl start mongod.service

# Then check the service’s status.
sudo systemctl status mongod

# After confirming that the service is running as expected, enable the MongoDB service to start up at boot:
sudo systemctl enable mongod

# The following command will connect to the database and output its current version, server address, and port.
mongo --eval 'db.runCommand({ connectionStatus: 1 })'
```



```bash
# coursera/setup.sh at master · words-sdsc/coursera
# https://github.com/words-sdsc/coursera/blob/master/big-data-3/mongodb/setup.sh

# create directory for database
mkdir db

# extract data
tar xzf dump.tar.gz

# import data
mongorestore dump
```



## Links

* [How To Install MongoDB on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04)
