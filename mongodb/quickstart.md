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



```shell
# change to the mongodb directory and run the MongoDB shell so that we can query the server.
cd /home/zhs2si/projects/bigdata/big-data-3/mongodb
mongo

# Run the show dbs command to see the databases:
show dbs 

# Let's switch to that database by running the use command:
use sample

# We can see the collections in the sample database by running show collections:
show collections

# We can run db.users.count() to count the number of documents:
db.users.count()

# Look at document and find distinct values. We can examine the contents of one of the documents by running db.users.findOne():
db.users.findOne()

# We can find the distinct values for a specific field by using the distinct() command. For example, let's find the distinct values for user_name:
db.users.distinct("user_name")

# Search for specific field value. We can search for fields with a specific value using the find() command. For example, let's search for user_name with the value ActionSportsJax:
db.users.find({user_name: "ActionSportsJax"})

# By appending .pretty() to the end of the find command, the results will be formatted:
db.users.find({user_name: "ActionSportsJax"}).pretty()

# Filter fields returned by query.We can specify a second argument to the find() command to only show specific field(s) in the result. Let's repeat the previous search, but only show the tweet_ID field:
db.users.find({user_name: "ActionSportsJax"}, {tweet_ID: 1})

# The _id field is primary key for every document, and we can remove it from the results with the following filter:
db.users.find({user_name: "ActionSportsJax"}, {tweet_ID: 1, _id:0})

# Perform regular expression search. MongoDB also supports searching documents with regular expressions. If we search for the value FIFA in the tweet_text field, there are no results:
db.users.find({tweet_text: "FIFA"})

# However, if we search using a regular expression, there are many results:
db.users.find({tweet_text: /FIFA/})

# We can append .count() to the command to count the number of results:
db.users.find({tweet_text: /FIFA/}).count()

# Search using text index. A text index can be created to speed up searches and allows advanced searches with $text. Let's create the index using createIndex():
db.users.createIndex({"tweet_text": "text"})
# The argument tweet_text specifies the field on which to create the index.

# we can use the $text operator to search the collection. We can perform the previous query to find the documents containing FIFA:
db.users.find({$text: {$search: "FIFA"}}).count()

# We can also search for documents not containing a specific value. For example, let's search for documents containing FIFA, but not Texas:
db.users.find({$text: {$search: "FIFA -Texas"}}).count()

# Search using operators. MongoDB can also search for field values matching a specific criteria. For example, we can find where the tweet_mentioned_count is greater than six:
db.users.find({tweet_mentioned_count: {$gt: 6}})

# The $gt operator search for values greater than a specific value. We can use the $where command to compare between fields in the same document. For example, the following searches for tweet_mentioned_count is greater than tweet_followers_count:
db.users.find({$where: "this.tweet_mentioned_count > this.tweet_followers_count"}).count()
# Note that the field names for $where are required to be prefixed with this, which represent the document.

# We can combine multiple searches by using $and. For example, let's search for tweet_text containing FIFA and tweet_mentioned_count greater than four:
db.users.find({$and: [{tweet_text: /FIFA/}, {tweet_mentioned_count: {$gt: 4}}]}).count()
```



## Links

* [How To Install MongoDB on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04)
* [Querying Documents in MongoDB | Coursera](https://www.coursera.org/learn/big-data-integration-processing/supplement/R93cO/querying-documents-in-mongodb)
