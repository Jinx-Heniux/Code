# Quickstart



```shell
# Install Apache Spark on Ubuntu 20.04
# Update system packages
sudo apt update
sudo apt -y upgrade

# Install Java with other dependencies
# Jave that is the requirement of Apache Spark along with some other things – Git and Scala to extend its capabilities.
sudo apt install curl mlocate default-jdk -y

# Verify Java installation.
java -version

# Download Apache Spark
# https://spark.apache.org/downloads.html
wget https://dlcdn.apache.org/spark/spark-3.2.1/spark-3.2.1-bin-hadoop3.2.tgz

# Extract the Spark tarball.
sudo tar xvf spark-3.2.1-bin-hadoop3.2.tgz

# Create an installation directory /opt/spark
sudo mkdir /opt/spark

# Move the extracted files to the installation directory.
sudo mv spark-3.2.1-bin-hadoop3.2/* /opt/spark
# or: sudo tar -xf spark*.tgz -C /opt/spark --strip-component 1

# Change the permission of the directory.
sudo chmod -R 777 /opt/spark

# Edit the bashrc configuration file to add Apache Spark installation directory to the system path.
# sudo vim ~/.bashrc
# or
# sudo nano ~/.bashrc

# Add the code below at the end of the file, save and exit the file:
# export SPARK_HOME=/opt/spark
# export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin

# Add Spark folder to the system path
echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.bashrc
echo "export PYSPARK_PYTHON=/usr/bin/python3" >> ~/.bashrc

# Save the changes to take effect.
source ~/.bashrc

# Start the standalone master server.
# start-master.sh # this doesn't work
/opt/spark/sbin/start-master.sh
# output looks like this
# starting org.apache.spark.deploy.master.Master, logging to /opt/spark/logs/spark-root-org.apache.spark.deploy.master.Master-1-ubuntu.out

# The process will be listening on TCP port 8080.
sudo ss -tunelp | grep 8080

# in browser open http://127.0.0.1:8080.
# Spark Master at spark://zhs2si.ubuntu.20.04.vm:7077 

# Start the Apache Spark worker process.
# ./start-slave.sh spark://zhs2si.ubuntu.20.04.vm:7077
# This script is deprecated, use start-worker.sh
/opt/spark/sbin/start-worker.sh spark://zhs2si.ubuntu.20.04.vm:7077
# output looks like this
# starting org.apache.spark.deploy.worker.Worker, logging to /opt/spark/logs/spark-zhs2si-org.apache.spark.deploy.worker.Worker-1-zhs2si.out

# Use the spark-shell command to access Spark Shell
/opt/spark/bin/spark-shell

# If you’re more of a Python person, use pyspark.
/opt/spark/bin/pyspark

# Easily shut down the master and slave Spark processes using commands below.
$SPARK_HOME/sbin/stop-worker.sh
$SPARK_HOME/sbin/stop-master.sh
```



## Links

* [Installing Apache Spark on Ubuntu 20.04 or 18.04 - Linux Shout](https://www.how2shout.com/linux/installing-apache-spark-on-ubuntu-20-04-or-18-04/)
* [Install Apache Spark on Ubuntu 22.04|20.04|18.04 | ComputingForGeeks](https://computingforgeeks.com/how-to-install-apache-spark-on-ubuntu-debian/)
* [Install Apache Spark on Ubuntu 20.04 - Vultr.com](https://www.vultr.com/docs/install-apache-spark-on-ubuntu-20-04/)

