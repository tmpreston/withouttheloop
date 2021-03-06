---
title: dbc type assertion library
author: Liam McLennan
date: 2013-07-16 20:32
template: article.jade
---

Today I updated my javascript dbc assertion library. I wanted to be able to guarantee that javascript objects match certain type constraints. Imagine a backbone view:

```javascript
var View = Backbone.View.extend({

    render: function () {
        this.$el.html(this.model.get('a') + ' ' + this.model.get('b'));
    }

});
```

We can see from inspection that this view will fail to render if its model does not have an `a` and a `b` property. `dbc` can improve this as follows:

```javascript
var View = Backbone.View.extend({
    initialize: function () {
        dbc.check(this.model.toJSON(), {
            a: [{validator: 'required'}],
            b: [{validator: 'required'}]
        });
    },
    render: function () {
        this.$el.html(this.model.get('a') + ' ' + this.model.get('b'));
    }
});
```

It will still fail, but now it will fail when the view is created, and it will be obvious that the failure was caused by bad data. 

The Readme
==========

`dbc` is a small library for design-by-contract defensive coding in javascript. 

It focuses especially on type assertions in an attempt to provide a small compensation for JavaScript's unfortunate dynamicness.

Have a function that requires a numeric argument? Try:

```javascript
function (num) {
    dbc.type(num, 'number');
    // the rest of your function
}
```

or if the function argument is an object, and you want to collect the validation results:

```javascript
function (myComplexObject) {
    var messages = dbc.validate(myComplexObject, {
        firstProp: [{validator: 'required'}]
    });
    // the rest of your function
}
```        

Features
--------

Check a value:

```javascript
    dbc.type(1, 'number', 'optional error message');
    dbc.isFunction(function () {}, 'optional error message');
    dbc.functionArity(function () {}, 0, 'optional error message');
    // etc
```

Check an object:

```javascript
    dbc.check({
        a: 1,
        b: "cat in the hat",
        c: function (f, s) { return f + s; }
    }, {
        a: [
            {validator: 'required', args: ['optional error message']}, 
            {validator: 'type', args: ['number']}, 
            {validator: 'custom', args: [ function (v) { 
                // custom validator function should return a boolean
                return v > 0; 
            }]}
        ],
        b: [{validator: 'type', args: ['string']}],
        c: [{validator: 'isFunction'}, {validator: 'functionArity', args: [2, {message: 'This is a more advanced error object', field: 'c'}]}]
    });
```

Validate an object (same as check except that it returns an array of errors instead of throwing an exception at the first error):

```javascript
    dbc.validate({
        a: 1,
        b: "cat in the hat",
        c: function (f, s) { return f + s; }
    }, {
        a: [
            {validator: 'required', args: ['optional error message']}, 
            {validator: 'type', args: ['number']}, 
            {validator: 'custom', args: [ function (v) { 
                // custom validator function should return a boolean
                return v > 0; 
            }]}
        ],
        b: [{validator: 'type', args: ['string']}],
        c: [{validator: 'isFunction'}, {validator: 'functionArity', args: [2, {message: 'This is a more advanced error object', field: 'c'}]}]
    });
```

Test
----

    npm install -g jasmine-node
    jasmine-node --coffee spec/
