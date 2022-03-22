# Quickstart

```bash
# Ubuntu 20.04
# Step 1 — Installing PostgreSQL
sudo apt update
subasdo apt install postgresql postgresql-contrib
sudo systemctl start postgresql.service

# Step 2 — Using PostgreSQL Roles and Databases
sudo service postgresql start
sudo -i -u postgres
psql
# or
sudo -u postgres psql
```



```bash
# set permissions so postgres can read csv files
chmod 755 /home/zhs2si/projects/bigdata/big-data-3

# create postgres account for zhs2si user
sudo -u postgres psql -c "CREATE USER zhs2si"

# make zhs2si account admin
sudo -u postgres psql -c "ALTER USER zhs2si with superuser"

# create default db for zhs2si account
sudo -u postgres createdb zhs2si

# decompress csv files
gzip -d *.csv.gz

# set permissions so postgres can read csv files
chmod 644 *.csv

# create and load tables for hands on
psql -f setup/init-postgres.sql

```



```bash
# start the Postgres shell by running psql
psql

# View table and column definitions. We can list the tables in the database with the \d command
\d

# We can see the column definitions of the buyclicks table by running \d buyclicks:
\d buyclicks

# Query table. We can run the following command to view the contents of the buyclicks table
select * from buyclicks;

# Filter rows and columns. We can query only the price and userid columns with the following command:
select price, userid from buyclicks;
```



## Links

* [How To Install PostgreSQL on Ubuntu 20.04 \[Quickstart\] | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart)
* [Instructions for Downloading Hands On Datasets | Coursera](https://www.coursera.org/learn/big-data-integration-processing/supplement/r8sXi/instructions-for-downloading-hands-on-datasets)
* [https://github.com/words-sdsc/coursera/blob/master/big-data-3/setup.sh](https://github.com/words-sdsc/coursera/blob/master/big-data-3/setup.sh)
* [Querying Relational Data with Postgres | Coursera](https://www.coursera.org/learn/big-data-integration-processing/supplement/OORny/querying-relational-data-with-postgres)
