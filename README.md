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
##You should get output as 5002


#Task 3: Adding members to replica set

Now add two more members to the set. Use the 2/ and 3/ directories we created in homework 4.1. Run those two mongod’s on ports 27002 and 27003 respectively (the exact numbers could be different).

Remember to use the same replica set name as you used for the first member.

You will need to add these two new members to your replica set, which will initially have only one member. In the shell running on the first member, you can see your replica set status with

rs.status()
Initially it will have just that first member. Connecting to the other members will involve using

```
rs.add()
For example,
rs.add("localhost:27002")
```

Note that 'localhost' almost certainly won't work for you unless you have already set it as 'localhost' in the previous problem. If not, try using the name in the "members.name" field in the document you get by calling rs.status(), but remember to use the correct port!.

You'll know it's added when you see an

{ "ok" : 1 }
document. Once a secondary has spun up, you can connect to it with a new mongo shell instance. Use
```
rs.slaveOk()

```
to let the shell know you're OK with (potentially) stale data, and run some queries. You can also insert data on your primary and then read it out on your secondary.
Once you have two secondary servers, both of which have sync'd with the primary and are caught up, run (on your primary):

homework.c()

## You should get output as 5


# Task 4 : Removing a member from Replica set

We will now remove the first member (@ port 27001) from the set.

As a first step to doing this we will shut it down. (Given the rest of the set can maintain a majority, we can still do a majority reconfiguration if it is down.)

We could simply terminate its mongod process, but if we use the replSetStepDown command, the failover may be faster. That is a good practice, though not essential. Connect to member 1 (port 27001) in the shell and run:

```
rs.stepDown()
```

Then cleanly terminate the mongod process for member 1.

Next, go to the new primary of the set. You will probably need to connect with the mongo shell, which you'll want to run with '--shell replication.js' since we'll be getting the homework solution from there. Once you are connected, run rs.status() to check that things are as you expect. Then reconfigure to remove member 1.

Tip: You can either use rs.reconfig() with your new configuration that does not contain the first member, or rs.remove(), specifying the host:port of the server you wish to remove as a string for the input.

When done, run:
> homework.d()

## You should get output as 6
