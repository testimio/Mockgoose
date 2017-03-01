[![Build Status](https://travis-ci.org/mccormicka/Mockgoose.png?branch=master)](https://travis-ci.org/mccormicka/Mockgoose)

Please Share on Twitter if you like #mockgoose
<a href="https://twitter.com/intent/tweet?hashtags=mockgoose&amp;&amp;text=Check%20out%20this%20%23Mongoose%20%23MongoDB%20Mocking%20Framework&amp;tw_p=tweetbutton&amp;url=http%3A%2F%2Fbit.ly%2F19gcHwm&amp;via=omnipitence" style="float:right">
<img src="https://raw.github.com/mccormicka/Mockgoose/master/twittershare.png">
</a>

## What is Mockgoose?

Mockgoose provides test database by spinning up mongod on the back when mongoose.connect call is made. By default it is using in memory store which does not have persistence.

## Install
To install the latest official version, use NPM:

    npm install mockgoose --save-dev


## Usage
You simply require Mongoose and Mockgoose and wrap Mongoose with Mockgoose.

```javascript
	var mongoose = require('mongoose');
	var Mockgoose = require('mockgoose').Mockgoose;
	var mockgoose = new Mockgoose(mongoose);

	mockgoose.prepareStorage().then(function() {
		// mongoose connection		
	});
```

Once Mongoose has been wrapped by Mockgoose connect() will be intercepted by Mockgoose so that no MongoDB instance is created.

## Mocha

```javascript
var Mongoose = require('mongoose').Mongoose;
var mongoose = new Mongoose();

var Mockgoose = require('mockgoose').Mockgoose;
var mockgoose = new Mockgoose(mongoose);

before(function(done) {
	mockgoose.prepareStorage().then(function() {
		mongoose.connect('mongodb://example.com/TestingDB', function(err) {
			done(err);
		});
	});
});

describe('...', function() {
	it("...", function(done) {
		// ...
		done();
	});
});
```

ES6

```javascript
import * as mongoose from 'mongoose';
import {Mockgoose} from 'mockgoose';

let mockgoose: Mockgoose = new Mockgoose(mongoose);

mockgoose.prepareStorage().then(() => {
	mongoose.connect('mongodb://foobar/baz');
	mongoose.connection.on('connected', () => {  
	  console.log('db connection is now open');
	}); 
});
```

## Helper methods and variables (mockgoose.helper)

### reset(callback)
Reset method will remove **ALL** of the collections from a temporary store,
note that this method is part of **mockgoose** object, and not defined under
**mongoose**

```javascript
mockgoose.helper.reset().then(() => {
	done()
});
```

### isMocked
Returns **TRUE** from **mongoose** object if Mockgoose is applied

```javascript
if ( mockgoose.helper.isMocked() === true ) {
  // mongoose object is mocked
}
```

## Development

This section contains instructions for developers working on the Mockgoose codebase.
It is not relevant if you just want to use Mockgoose as a library in your project.

### Pre-requisites

* Node.js >= 0.10.0

### Setup

```
git clone git@github.com:mockgoose/Mockgoose.git
cd Mockgoose
npm install
npm test
```
