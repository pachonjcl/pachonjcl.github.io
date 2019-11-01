---
layout: post
title:  "React Hooks Part 2"
date:   2019-10-30 20:39:10 -0400
categories: react
---
In the last post, It was shown the usage of `useState` hook. Although it was just
a very small example, it let us show that with hooks we can use React lifecycle inside
functions and make the code cleaner.

As a convention, all the hooks must start with `use`, that's why last post we used
`useState` hook, so now let's talk about `useEffect`. Effects are external interactions
such as network request, web socket messages, time-based events, etc.

Let's take a look at a common scenario of a component loading data from api at the begining
of the component loading in the old fashion way. Let's add a `todo` array to the state and
load it with `fetch` on `componentDidMount`. So far nothing new.

{% highlight JSX %}
import React from 'react';

const url = 'https://raw.githubusercontent.com/pachonjcl/pachonjcl.github.io/master/data/todos.json'

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0, todos: []
    };
  }

  componentDidMount() {
    fetch(url)
      .then(res => res.json())
      .then(({ todos }) => this.setState({ todos }))
  }

  render() {
    const { count, todos } = this.state;
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => this.setState({ count: count + 1 })}>
          Click me
        </button>
        <br /><br /><br />
        <div>
          {
            todos.map(todo =>
              <div key={todo.id}> {todo.text} </div>
            )
          }
        </div>
      </div>
    );
  };
}
{% endhighlight %}

Now let's see the equivalent of that with React hooks. As I have mentioned before `useEffect` is
meant to handle external interactions, one of them is getting data with a request. But let's talk
about what parameters is `useEffect` expecting. First of all `useEffect` receives two parameter 
the first one is `effectCallback` wich is basically the function that will initiate the external
interaction and the second one is a optional `dependencies` array, that in a nutshell whenever changes
any of its elements will make the `effectCallback` to be called.

Let's see the code first and after that we will discuss it's meaning in the example.

{% highlight JSX %}
import React, { useState, useEffect } from 'react';

const url = 'https://raw.githubusercontent.com/pachonjcl/pachonjcl.github.io/master/data/todos.json'

function App() {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(({ todos }) => setTodos(todos))
  }, [])

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
      <br /><br /><br />
      <div>
        {
          todos.map(todo =>
            <div key={todo.id}> {todo.text} </div>
          )
        }
      </div>
    </div>
  );
}

{% endhighlight %}

Ok, the most interesting part of the code is the `useEffect` hook call, in there we pass a
callback function wich is doing the external interaction by fetching data from a "endpoint" and
when data is fetched sets the data to the `todos` array.

**But why are we using an empty array of `dependencies`?**

The following scenarios will illustrate what could happen:

1. Since `dependencies` is an optional parameter, if we don't pass it to `useEffect` it will make call
`effectCallback` on every render cycle. Try it yourself, because the effect is modifying the state it will make call the effect again and again, wich will result in a infinite loop.
2. If we pass an empty array it will make call `effectCallback` only once. This is the equivalent of `componentDidMount`.
3. If we pass `count` inside dependencies array it will make call `effectCallback` every time `count`
changes. Again, try it yourself to see what happens.

There remains a couple of things to be discussed on the next post:
* What if the component is being unmounted while the request is being fetched?.
* How to reproduce the `componentDidUpdate` or `componentWillUnmount`.
* How to use multiple effects in a component?.

I will continue writing about React in the next part of this React Hooks Series.

Check out the [React Hooks docs][react-docs] for more info on how to get the most out of React Hooks.

[react-docs]: https://reactjs.org/docs/hooks-intro.html
