###Note: if Mongodb 3.0, pls configure security.authorization=enabled / disabled instaed.

1. Launch without --auth
2. Change auth schema to 3 first:
  * use admin
  * db.system.users.remove({})
  
  * // create admin user of all users
  * db.createUser({user: 'admin', pwd: 'admin-pass', roles: [{role: 'userAdminAnyDatabase', db: 'admin'}]})
  * 
  * var schema = db.system.version.findOne({"_id": "authSchema"}); schema.currentVersion = 3;
  * db.system.version.save(schema);
  
3. Launch with --auth
  * use ourDB
  * db.dropUser("aaa")
  * db.createUser({user: '', pwd: '', roles: [{role: 'readWrite', db: 'ourDB'}]})
4. use ourDB
5. db.auth('', '')


#### issues

* quit by 'kill' command or other reason, pls use follow commands:
  * mongod --repair
* 

