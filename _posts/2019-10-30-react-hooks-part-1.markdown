---
layout: post
title:  "React Hooks Part 1"
date:   2019-10-30 20:39:10 -0400
categories: react
---
Starting from React 16.8 Hooks has been added to allow use React features without
writing classes. But, what does it means?

Let's check the Ol' Reliable `counter` example with class components:

{% highlight JSX %}
import React from 'react';

class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    const { count } = this.state;
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => this.setState({ count: count + 1 })}>
          Click me
        </button>
      </div>
    );
  };
}
{% endhighlight %}

Now, let's see what is the equivalent with React Hooks:

{% highlight JSX %}
import React, { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
{% endhighlight %}

There are some important points to note there:

* State variables are defined with help `useState` hook and destructuring assignment.

  * The parameter that is being send to `useState` is the initial value for state.
  * The returned value of `useState` is a two element array, that's why it is being destructured 
as a name of variable and its setter, in this case `count` and `setCount`.

* Now with hooks we don't need to define a class and its constructor, neither need to be
tied to `this.state` or `this.setState(...)`.

* Code looks much cleaner :).

This is just the tip of the iceberg, I will continue writing about React in the next part
of this React Hooks Series.

Check out the [React Hooks docs][react-docs] for more info on how to get the most out of React Hooks.

[react-docs]: https://reactjs.org/docs/hooks-intro.html
