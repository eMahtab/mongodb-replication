# mongodb-replication
Creating MongoDB Replica Set

1. We will create a three member replica set. Pick a root working directory to work in. Go to that directory in a console window.

Given we will have three members in the set, and three mongod processes, create three data directories:
```
mkdir 1
mkdir 2
mkdir 3
```

2. We will now start a single mongod as a standalone server. Given we will have three mongod processes on our single test server, we will explicitly specify the port numbers (this wouldn’t be necessary if we had three real machines or three virtual machines). We’ll also use the --smallfiles parameter and --oplogSize so the files are small given we have a lot of server processes running on our test PC.

# Task 1- starting as a standalone server for problem 1:
```
mongod --dbpath 1 --port 27001 --smallfiles --oplogSize 50
```
Note: for all mongod startups in the homework this chapter, you can optionally use --logPath, --logappend, and --fork. Or, since this is just an exercise on a local PC, you could simply have a separate terminal window for all and forgo those settings. Run “mongod --help” for more info on those.

4. In a separate terminal window (cmd.exe on Windows), run the mongo shell with the replication.js file:

mongo --port 27001 --shell replication.js
Then run in the shell:

homework.init()
This will load a small amount of test data into the database.

Now run:

homework.a() and enter the result. This will simply confirm all the above happened ok.

##You should get output as 5001


#Task 2 : Creating single server replica set

Now convert the mongod instance (the one in the problem 4.1 above, which uses “--dbpath 1”) to a single server replica set. To do this, you’ll need to stop the mongod (NOT the mongo shell instance) and restart it with “--replSet” on its command line. Give the set any name you like.

Then go to the mongo shell. Once there, run

```
rs.initiate()
```
Note: if you do not specify a configuration, the mongod will pick one based on your computer's hostname.

When you first ran homework.init(), we loaded some data into the mongod. You should see it in the replication database. You can confirm with:

```
use replication
db.foo.find()
```

Once done with that, run

homework.b() in the mongo shell, what output do you get 
