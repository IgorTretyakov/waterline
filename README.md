# [<img title="waterline-logo" src="http://i.imgur.com/3Xqh6Mz.png" width="610px" alt="Waterline logo"/>](http://waterlinejs.org)

[![NPM version](https://badge.fury.io/js/waterline.svg)](http://badge.fury.io/js/waterline)
[![Master Branch Build Status](https://travis-ci.org/balderdashy/waterline.svg?branch=master)](https://travis-ci.org/balderdashy/waterline)
[![Master Branch Build Status (Windows)](https://ci.appveyor.com/api/projects/status/tdu70ax32iymvyq3?svg=true)](https://ci.appveyor.com/project/mikermcneil/waterline)
[![StackOverflow (waterline)](https://img.shields.io/badge/stackoverflow-waterline-blue.svg)]( http://stackoverflow.com/questions/tagged/waterline)
[![StackOverflow (sails)](https://img.shields.io/badge/stackoverflow-sails.js-blue.svg)]( http://stackoverflow.com/questions/tagged/sails.js)

Waterline is a brand new kind of storage and retrieval engine.

It provides a uniform API for accessing stuff from different kinds of databases, protocols, and 3rd party APIs. That means you write the same code to get and store things like users, whether they live in Redis, MySQL, MongoDB, or Postgres.

Waterline strives to inherit the best parts of ORMs like ActiveRecord, Hibernate, and Mongoose, but with a fresh perspective and emphasis on modularity, testability, and consistency across adapters.

For detailed documentation, see [the Sails documentation](http://sailsjs.com).


> Looking for the version of Waterline used in Sails v0.12?  See https://github.com/balderdashy/waterline/tree/0.11.x.

## Installation
Install from NPM.

```bash
  $ npm install waterline
```

## Overview
Waterline uses the concept of an adapter to translate a predefined set of methods into a query that can be understood by your data store. Adapters allow you to use various datastores such as MySQL, PostgreSQL, MongoDB, Redis, etc. and have a clear API for working with your model data.

Waterline supports [a wide variety of adapters](http://sailsjs.com/documentation/concepts/extending-sails/adapters/available-adapters) both core and community maintained.

## Help
Need help or have a question?  Click [here](http://sailsjs.com/support).


## Bugs &nbsp; [![NPM version](https://badge.fury.io/js/waterline.svg)](http://npmjs.com/package/waterline)
To report a bug, [click here](http://sailsjs.com/bugs).


## Contribute
Please observe the guidelines and conventions laid out in our [contribution guide](http://sailsjs.com/documentation/contributing) when opening issues or submitting pull requests.

#### Tests
All tests are written with [mocha](https://mochajs.org/) and should be run with [npm](https://www.npmjs.com/):

``` bash
  $ npm test
```

## Meta Keys

As of Waterline 0.13 (Sails v1.0), these keys allow end users to modify the behaviour of Waterline methods. You can pass them as the `meta` query key, or via the `.meta()` query modifier method:

```javascript
SomeModel.create({...})
.meta({
  skipAllLifecycleCallbacks: true
})
.exec(...);
```

These keys are not set in stone, and may still change prior to release. (They're posted here now as a way to gather feedback and suggestions.)



Meta Key                              | Default         | Purpose
:------------------------------------ | :---------------| :------------------------------
cascade                               | false           | Set to `true` to automatically "empty out" (i.e. call `replaceCollection(..., ..., [])`) on plural ("collection") associations when deleting a record.  _Note: In order to do this when the `fetch` meta key IS NOT enabled (which it is NOT by default), Waterline must do an extra `.find().select('id')` before actually performing the `.destroy()` in order to get the IDs of the records that would be destroyed._
fetch                                 | false           | For adapters: When performing `.update()` or `.create()`, set this to `true` to tell the database adapter to send back all records that were updated/destroyed.  Otherwise, the second argument to the `.exec()` callback is `undefined`.  Warning: Enabling this key may cause performance issues for update/destroy queries that affect large numbers of records.
skipAllLifecycleCallbacks             | false           | Set to `true` to prevent lifecycle callbacks from running in the query.
skipRecordVerification                | false           | Set to `true` to skip Waterline's post-query verification pass of any records returned from the adapter(s).  Useful for tools like sails-hook-orm's automigrations.  **Warning: Enabling this flag causes Waterline to ignore `customToJSON`!**
skipExpandingDefaultSelectClause      | false           | Set to `true` to force Waterline to skip expanding the `select` clause in criteria when it forges stage 3 queries (i.e. the queries that get passed in to adapter methods).  Normally, if a model declares `schema: true`, then the S3Q `select` clause is expanded to an array of column names, even if the S2Q had factory default `select`/`omit` clauses (which is also what it would have if no explicit `select` or `omit` clauses were included in the original S1Q.) Useful for tools like sails-hook-orm's automigrations, where you want temporary access to properties that aren\'t necessarily in the current set of attribute definitions.  **Warning: Do not use this flag in your web application backend-- or at least [ask for help](https://sailsjs.com/support) first.**


#### Related model settings

To provide per-model/orm-wide defaults for the `cascade` or `fetch` meta keys, there are a few different model settings you might take advantage of:

```javascript
{
  attributes: {...},
  primaryKey: 'id',

  skipCascadeOnDestroy: true,

  fetchRecordsOnUpdate: true,
  fetchRecordsOnDestroy: true,
  fetchRecordsOnCreate: true,
  fetchRecordsOnCreateEach: true,
}
```

> Not every meta key will necessarily have a model setting that controls it-- in fact, to minimize peak configuration complexity, most will probably not.


## License
[MIT](http://sailsjs.com/license). Copyright © 2012-2017 Mike McNeil, Balderdash Design Co., & The Sails Company

Waterline, like the rest of the [Sails framework](http://sailsjs.com), is free and open-source under the [MIT License](http://sailsjs.com/license).


![image_squidhome@2x.png](http://sailsjs.com/images/bkgd_squiddy.png)
