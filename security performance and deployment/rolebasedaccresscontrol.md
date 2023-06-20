## role based access control

Roles never limit privileges. If a user has two roles, the role with the greater access takes precedence.
For example, if you grant the read role on a database to a user that already has the readWriteAnyDatabase role, the read grant does not revoke write access on the database.To revoke a role from a user, use the revokeRolesFromUser command.


## creating a user

we need to configure the server and start it so that only authenticated users can manupulate the database a

The command "mongod --auth" is used to start the MongoDB server with authentication enabled. Here's what it does:<br>

Starts the MongoDB server: The "mongod" command is used to start the MongoDB server.<br>

Enables authentication: The "--auth" flag is provided to enable authentication for client connections to the MongoDB server. When authentication is enabled, clients must provide valid credentials (username and password) to connect and perform operations on the server.<br>

now after this if we wanna show the dbs and manipulate we cannot

so there is one user we can create now that is the admin user

## how to create a user admin

to create a user admin we need to naviagte to the admin database

```js
use admin
```
and after switching to admin we can create a new user 

```js
db.createUser({user:"manish", pwd:"password", roles:["userAdminAnyDatabase"]})
```

now after we create a user we can use the role and credential to manupulate the database<br>
to authencicate using the username we can use
```js
db.auth('manish','password')

//another way of authenticate using the --authencitateDatabse
mongosh -u username -p password --authenticationDatabse databasename
```
now after that we are we are authincated and now can perform operations on the database


## Roles

Database user - read, readwrite<br>
Database  Admin - dbAdmin, userAdmin, dbOwner<br>
All Database Roles - readAnyDatabase, readWriteAnyDatabase, userAdminAnyDatabase, dbAdminAnyDatabase<br>
Cluster Admin - clusterManager, clusterMonitor,hostManager,clusterAdmin<br>
backup/restore - backup, restore<br>
superUser- dbOwner, userAdmin, userAdminAnyDatabase, root



## updating and extending roles to other database

so we wanna change something to the user cause now our user is  only able to read and write

the db.updateUser takes first argument the name of the user we want to update
the second document is the change to the user

examples:
 lets take an example where we want to change the role of user and also extend to other database

```js
db.updateUser(nameoftheuser, {role: ["readWrite", {role: "readWrite", db: "blog" }]})
```


## adding SSL transport encryption

