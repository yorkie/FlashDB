FlashDB
=======

## What's the flashDB
A database system for providing a local, fast, convenient accessor, which can query, extend and remove.  
And it's support Multi-keys.

## Background
As you know, you could use `memcached` for replacing `flashDB`.   
That's all right, It's powerful, sophisticated develop tool, but if you use flashDB, it's more light, more fast and more close to you.  
Here is a case, you need build an authenticate system, programmers should move the table `auth` or related stuff to the CPU/RAM of servers at the startup stage, so it's long for a local/fast database system especially a more complexed authenticate needed process.

## API, It's very KISS

### .create(name, keys)
```javascript
var flashDB = require('./flashDB')
var Persons = flashDB.create('Persons', ['firstName', 'lastName'])
```

### .add(value)
Only one param as you see, the reason is your key/keys is created after `.create()`, such that while you want to add a document/value, it can valid and recognize a certain number of keys and extracted them. To our surprise, `FlashDB` did as a different library that support multi-keys maps.
```javascript
Person.add({
  firstName: 'Yorkie',
  lastName:  'Nell',
  Age:       23
})
```

### .get(key, by)
```javascript
// namely getByDefaultKey()
var Yorkie = Person.get('Yorkie')
// namely getByFirstName()
var Delo   = Person.get('Yorkie', 'firstName')
// namely getByLastName()
var Nell   = Person.get('Nell', 'lastName')

// test
console.log( Yorkie === Nelo )  // true
console.log( Yorkie === Nell )  // true
console.log( Nelo   === Nell )  // true
```

## How to ensure that a set had existed in a map?
Answer is:
```javascript
// you can create a map like this:
var users = flashDB.create('Users', ['name', 'email'])
// push all user on startup
db.users.forEach(function(user) {
  users.add(user)
})
```
Next, you need code the answer in your register handler:
```javascript
function isExisted(user) {
  var users = flashDB.users
  return users.get(user.name, 'name') || users.get(user.email, 'email')
}

//you can call it:
var user = {
  name: 'yorkie',
  email: 'l900422@vip.qq.com'
}
if (isExisted(user)) {
  // error handler
}
// your turn
```
