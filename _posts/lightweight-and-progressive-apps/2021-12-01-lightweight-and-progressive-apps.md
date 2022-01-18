# 5. Lightweight and progressive apps

## Introduction

One challenge of building modern *web* applications is the minimal native support for reactivity. Unlike statically generated websites, web apps are in a constant state of flux, required to update views on the fly as state changes. While the recent introduction of *Web Components* has helped remedy the lack of native support, it’s still difficult to achieve the fluent reactivity typical of frameworks like React and Vue. It has now therefore become *de facto* to use such frameworks when building professional projects — and they work brilliantly. However, there are several downsides to using them:

- Overhead — your users’ browsers have to download, parse and execute large JavaScript resources which implement the framework, in addition to your app’s code. This can lead to noticeable overhead if care is not taken to optimize. There’s also additional development overhead when using frameworks: in the case of React and Vue, your app can end up with well over 500 dependencies. These slow down installation and compilation times.
- Security — it’s often the case a dozen or so vulnerabilities are introduced into your app inadvertently by dependencies. While many are often unexploitable, making the assumption that *all* are isn’t a good security practice. Note that in general frameworks *themselves* do provide good protection against common vulnerabilities like XSS.
- Dependence — the degree to which your app is up to date with current standards is effected by the maintenance of the framework and dependencies.

The first problem is most significant. For lower power devices and users with slow internet connections, a web app’s resource size can become significant. Consider for example, the Lighthouse report for [https://medium.com](https://medium.com) (popular blogging platform):

![Untitled](5%20Lightweight%20and%20progressive%20apps%201d4eda50b5fc4a79848ee4fc73082d06/Untitled.png)

![Untitled](5%20Lightweight%20and%20progressive%20apps%201d4eda50b5fc4a79848ee4fc73082d06/Untitled%201.png)

This low performance score is typical of many modern websites. While frameworks don’t always lead to bad scores, it’s far easier to introduce overhead when using a framework. 

As developers, we want our app to be accessible any user, even if their internet connection is very limited. Hence we outline the following requirements:

- Implementation of PWA — e.g. caching resources
- No server whatsoever — all of the application’s logic will be implemented client side. Our app is relatively simple and doesn’t require a large amount of business logic. This approach will mean internet connectivity is only a factor when initially downloading the app.
- No frameworks
- No analytics or third-party scripts
- Minimal UI — to reduce nested function calls and excessively deep DOM trees
- Compression of all resources

## Achieving Reactivity Without React

Our requirement to forgo using a framework left us in a slightly difficult position. We needed to somehow achieve modern features like encapsulation, modular components and data-binding, using only vanilla JavaScript. There were three sources of inspiration which lead us towards a solution.

### 1. Telegram K

The popular messaging app *Telegram* was originally released as a for mobile devices. Later they launched a web client. This web client was subsequently replaced by two competing modernised clients — one built with a reimplementation of React, and another in frameworkless JavaScript. The result of the competition was two highly reactive, performant web apps.

Of course, the frameworkless implementation (Telegram Web K) peaked our interest. Their approach was to implement components as functions that return HTML nodes. Each component defined necessary event listeners and logic, intermixed with explicitly constructing the component’s DOM. The main success of this approach was the easy composition of components. However, the approach wasn’t declarative, contributing to less readable code. They could’ve also achieved better separation of view and model code by using Web Components. Nonetheless, the project was an impressive demonstration of how native JavaScript *can* be used to build modern apps, with a small penalty on code quality.

![Untitled](5%20Lightweight%20and%20progressive%20apps%201d4eda50b5fc4a79848ee4fc73082d06/Untitled%202.png)

### 2. Flutter

A second, larger source of inspiration for us was Google’s multiplatform framework, *Flutter*. Flutter can compile an app written in Dart to native IOS, Android and JavaScript code. What we found inspirational, was their approach to building layouts declaratively. Here’s an example of a Flutter widget:

![Untitled](5%20Lightweight%20and%20progressive%20apps%201d4eda50b5fc4a79848ee4fc73082d06/Untitled%203.png)

Flutter widgets use the innate composability of functions to describe layouts declaratively. We realised the same idea could be easily translated to JavaScript. However, we were still lacking a solution to the reactivity problem. Consider the following react component:

![Untitled](5%20Lightweight%20and%20progressive%20apps%201d4eda50b5fc4a79848ee4fc73082d06/Untitled%204.png)

In this component, any changes to `state.clicks` will cause the button to re-render. Now, we could quite easily mimic this behaviour with Web Components by simply re-rendering whenever state is changed. The problem is that re-rendering *the whole* DOM for a component is inefficient and sometimes just plain wrong. Two examples:

- A component that displays chat room messages and needs to update whenever a new message arrives. Re-rendering the entire component would be incredibly inefficient.
- An input box that changes the state of its parent component. Re-rendering would cause the focus-state of the input box to be reset — i.e. every time you type a letter, your browser would unselect the input box.

So when using Web Components, it’s typically necessary to use imperative even listeners — not what we’re aiming for. Our solution is presented in the next section.

### 3. Observables

The solution we discovered, was to combine Flutter’s layout method with an OOP design pattern called *Observer*. In this pattern we have one *event source* that pushes events (i.e. data) to several observers. While a simple idea, it allows us to implement two-way data-binding.

Observer is usually implemented like so:

```jsx
class Observable {

		constructor() {
				this.observers = []
		}

		notify(event) {
				this.observers.forEach(observer => observer.observe(event))
		}

		subscribe(observer) {
				this.observers.push(observer)
		}
}
```

```jsx
class Observer {
		constructor(observe) {
				this.observe = observe
		}
}
```

Let’s test it out!

```jsx
const name = new Observable()
const upper = new Observer(text => console.log(text.toUpperCase()))
const lower = new Observer(text => console.log(text.toLowerCase()))

name.subscribe(upper)
name.subscribe(lower)

name.notify('hello') 

// "HELLO"
// "hello"

name.notify('wOrLd')

// "WORLD"
// "world"
```

To apply this concept to data-binding, we create a specialised observable type — a so called “observable variable.” This is an observable which maintains a value, and whenever its value changes, it notifies all observers/subscribers.

The final insight was then to make components take in observable variables *rather than* static JavaScript values. For example, to implement a simple text component, we could do the following:

```jsx
function Text(text) {

		const node = document.createElement('span')
		text.on(text => node.innerText = text)

		return node
}
```

```jsx
const name = variable('Alice')
const text = Text(name)

// <span>Alice</span>

// update 

name('Sam')

console.log(text)

// <span>Sam</span>
```

After this finding, we created a tiny 165 byte library to implement observable variables. From here we could build fully reactive *and declarative* views. In our app, a widget is defined as a function which takes a number of observable variables, and returns a HTML node object. 

## Recap

We have a simple declarative way to achieve modularity, encapsulation and reactivity, all without a single framework or library. Here’s an example of a reactive widget using our technique:

```jsx
function App() {
		const name = use('Alice')
		
		return div(
				div(f`hello my name is ${name}, nice to meet you.`),
				input(name)
		)
}
```

Compare with the equivalent React component:

```jsx
function App() {
		const [name, setName] = useState('Alice')

		return (
				<div>
						<div>hello my name is {name}, nice to meet you.</div>
						<input oninput={value => setName(value)}>
				</div>
		)
}
```