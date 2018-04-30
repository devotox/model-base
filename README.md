[![Build Status](https://travis-ci.org/devotox/model-base.svg)](http://travis-ci.org/devotox/model-base)
[![Coverage Status](https://codecov.io/gh/devotox/model-base/branch/master/graph/badge.svg)](https://codecov.io/gh/devotox/model-base)
[![NPM Version](https://badge.fury.io/js/model-base.svg)](http://badge.fury.io/js/model-base)
[![NPM Downloads](https://img.shields.io/npm/dm/model-base.svg)](https://www.npmjs.org/package/model-base)
[![Dependency Status](https://david-dm.org/poetic/model-base.svg)](https://david-dm.org/poetic/model-base)
[![DevDependency Status](https://david-dm.org/poetic/model-base/dev-status.svg)](https://david-dm.org/poetic/model-base#info=devDependencies)
[![Greenkeeper badge](https://badges.greenkeeper.io/devotox/model-base.svg)](https://greenkeeper.io/)

model-base
==============================================================================

Classes to  extend and help create useful data models with strict validation

Installation
------------------------------------------------------------------------------

```
yarn add model-base
```


Usage
------------------------------------------------------------------------------

```javascript
# /api/models/analytics.js

'use strict';

const Model = require('model-base').Model;
const ModelCollection = require('model-base').ModelCollection;

const AnalyticsAttributes = {
	id: {
		type: 'integer',
		required: true,
		description: 'analytics datum id'
	},
	metric: {
		type: 'string',
		required: true,
		description:'type of analytics datum'
	},
	value: {
		type:'float',
		required: true,
		description:'value of metric'
	}
};

class AnalyticsModel extends Model {
	constructor(values) {
		super(AnalyticsAttributes, values);
	}
}

class AnalyticsModelCollection extends ModelCollection {
	constructor(values, coerce) {
		super(AnalyticsModel, values, coerce);
	}
}

module.exports.AnalyticsModel = AnalyticsModel;
module.exports.AnalyticsAttributes = AnalyticsAttributes;
module.exports.AnalyticsCollection = AnalyticsModelCollection;
```

```javascript
# /api/controllers/analytics.js

'use strict';

let ModelFile = require(`/api/models/analytics`);
let ModelCollection = ModelFile.AnalyticsCollection;

let collection = new ModelCollection(result.rows, true);
return collection.validate().getValues(true);
```

### List Of Validation Types

```javascript
'array',
'buffer',
'string',
'boolean',
'ip',
'url',
'slug',
'uuid',
'fqdn',
'json',
'email',
'alpha',
'base64',
'hex',
'alpha_numeric',
'phone',
'function',
'date',
'time',
'datetime',
'daterange',
'timestamp',
'integer',
'float',
'number',
'object'
  ```


### List Of Attribute Options
- type (required) - type of validation function to run on value

- required (default: false) - states whether the value is required or not

- default_value - any value or function that returns a value that will be set if no value is present

- custom - array of custom validation functions that allow you to do further validations on a value
	- return a string value with the error if there is an error or nothing if no error

- transform - function to transform input into any output
	- must return a value or the value will be set to undefined

- choices - array of possible values for model attribute
	- if the value is not one of the choices an error will be thrown

- alias - key for other model attribute that keeps the values the same
	- it passes the value from the other attribute into this one


Contributing
------------------------------------------------------------------------------

### Installation

* `git clone <repository-url>`
* `cd model-base`
* `yarn install`

### Linting

* `yarn lint`

### Running tests

* `yarn test` – Runs the test suite on the current Node Version

License
------------------------------------------------------------------------------

This project is licensed under the [MIT License](LICENSE.md).
