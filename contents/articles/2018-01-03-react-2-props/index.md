---
title: Learning React 2 - Data in with props
author: Liam McLennan
date: 2018-01-03 20:32
template: article.jade
---

In [Learning React 1 - A first component](/articles/2018-01-03-react-1/) I showed how to bootstrap a React application and implement a simple component that displays the current date.

The `Today` component is highly specialized. It displays the current date, but can't do anything else. Often we would like to generalize a component to increase the opportunities for reuse. This requires the capability to pass data into a React component. 

> Data is passed to a React component using 'props'

Props are specified when the component is used, as attributes. We can pass a date to the `Today` component like this:

```javascript
ReactDOM.render(<Today day={new Date(2018, 2, 20)} />, document.getElementById('root'));
```

Then in the `Today` component this value is available on the props object passed to the function:

```javascript
function Today(props) {
    return <div>Today is {props.day.toString()}</div>;
}
```

Now the output is:

```
Today is Tue Mar 20 2018 00:00:00 GMT+1000 (AEST)
```

If you are wondering why it is Mar 20 and not Feb 20, it is because [JavaScript uses 0 based indexing for months](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date).

When reading other peoples React code you will often see [Object destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) used to show what is expected in `props`:

```javascript
function Today({ day }) {
    return <div>Today is {day.toString()}</div>;
}
```

This is a purely syntactical change. The program behaves exactly the same as before. 

Summary
======

By introducing `props` to our React component we have generalized it to work with any date. This is the mechanism by which data is passed to React components, and passed down a hierarchy of React components. 