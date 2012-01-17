waitlist
========

Manage consumers standing in a queue for limited resources.

example
=======

``` js
var waitlist = require('waitlist');
var EventEmitter = require('events').EventEmitter;

var ws = waitlist();

ws.add('beep', { id : 0 });
ws.add('boop', { id : 1 });

(function consume (userId) {
    setTimeout(function () {
        var em = new EventEmitter;
        em.on('spot', function (n) {
            console.log('user ' + userId + ' in spot ' + n);
        });
        
        em.on('available', function (res) {
            console.log('user ' + userId
                + ' using resource ' + JSON.stringify(res)
            );
        });
        
        em.on('expire', function () {
            console.log('user ' + userId + ' expired');
        });
        
        ws.acquire(Math.random() * 1000, em.emit.bind(em));
        
        consume(userId + 1);
    }, Math.random() * 500);
})(0)
```

output:

```
$ node examples/shared.js 
user 0 using resource {"id":0}
user 1 using resource {"id":1}
user 2 in spot 1
user 2 in spot 1
user 3 in spot 2
user 0 expired
user 2 using resource {"id":0}
user 3 in spot 1
user 3 in spot 1
user 4 in spot 2
user 3 in spot 1
user 4 in spot 2
user 5 in spot 3
user 1 expired
user 3 using resource {"id":1}
user 4 in spot 1
user 5 in spot 2
^C
```

methods
=======



install
=======

With [npm](http://npmjs.org) do:

```
npm install waitlist
```

[![build status](https://secure.travis-ci.org/substack/node-waitlist.png)](http://travis-ci.org/substack/node-waitlist)
