# 4. UI Compression

## Introduction

With designs in place, our first goal is define a set of reusable components which implement the UI. Consider the following hypothetical HTML code:

```html
<div class='nav'>
  <ul>
    <li><a href=#home>Home</a></li>
    <li><a href=#profile>Profile</a></li>
    <li><a href=#messages>Messages</a></li>
  </ul>
</div>
<div class='main'>
  <h1>Recent messages (1)</h1>
  <div class='messages'>
    <ul>
      <li>
        <div class='card'>
          <div class='card-image'>
            <img src='/static/uploads/images/users/382341948.png'>
          </div>
          <div class='card-content'>
            <h3>Hey how are you?</h3>
          </div>
        </div>
      </li>
      <li>
        ...
      </li>
    </ul>
  </div>
```

This gives a verbose description of a “recent messages” view. We can drastically reduce code volume by taking into account redundancies. There are three main types to consider:

- Visual — common styles used by different elements. For example, a particular font size might be used in several places. In these cases, we should not specify it every time but rather abstract it to a general style rule.
- Functional — common behaviour used by different elements. In the example above, both the navigation menu and “recent messages” section generate an unordered list, from some data. Instead of writing code to do this for each instance, we should make a general purpose function.

If two widgets are visually and functionally identical, we say they’re the same component. If they have slight differences, we consider them *variants* of the same component. Using components has the following benefits:

- efficiency — by removing redundancies, there is less CSS, less JavaScript and less HTML for browsers to download, parse and execute. Overall this contributes towards more a efficient and responsive UI.
- intelligibility  — the UI becomes much easier to reason about when using a component-based taxonomy. If we know how a component type should behave in general, we can be confident about how each instance of it will behave. Bugs and security issues are also far easier to identify and fix.
- maintainability — the code can be updated in less time because we may only need to change one or two components — rather than every instance of that component. We can also readily swap out old components for new ones, hence keeping the system up to date.
- modular development — developers can work on induvial parts of the system without needing to know the internals of everything else. If they need a certain feature, they can just import it.
- code beauty — the code becomes more concise and declarative. This helps code clarity, which in turn speeds up processes like debugging.

What we aim for is a *minimal* component set. That is, a set of components (and variants) which describes the UI in the least amount of code. This generally maximizes the reward from the benefits above

## First Draft

To create a minimal set, we start by simply observing our UI. We need to identify all of the commonalities and differences between widgets in the design.

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2012.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%201.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%202.png)

### Redundancies

(1.1) An image horizontally centred above a piece of text. Parameters (image widget, text widget)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%203.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%204.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%205.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%206.png)

(1.2) Padding around sections.

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%207.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%208.png)

(1.3) Icons next to text and a `>` symbol. Parameters (icon widget, text widget, whether to show `>`)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%209.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2010.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2011.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2012.png)

(1.4) Various standard text sizes. Parameters (the type of text (e.g. heading, copy))

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2013.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2014.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2015.png)

(1.5) Rounded box containing other widgets. Parameters (the child widgets)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2016.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2017.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2018.png)

(1.6) Circular images. Parameters (the image)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2019.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2020.png)

(1.7) Lists with dividers. Parameters (the child widgets)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2021.png)

(1.8) An instance of (1.5) with (1) inside

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2022.png)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2023.png)

(1.9) The recommendation cards. Parameters (category, description)

![Untitled](/assets/4%20UI%20Compression%201f74894286de4fbc911e33a70271ddd9/Untitled%2024.png)

## Second Draft

In our first draft we were able to find several repeated patterns throughout the UI. We can further compress the UI by finding redundancies in the *groups* of patterns.

### Redundancies

(2.1) Instances of (1.3) are exclusively found within instances of (1.5). Combine them. Now (1.9) looks very similar to (2.1) so combine them. Parameters (icon, title, copy, whether to show `>`)

(2.2) If we add (whether to show a background) as a parameter to (1.5), then (1.1) are exclusively found within (1.5). So combine them. Parameters (icon, text)

### Components

After this further round of compression, we’re left with a quite optimal set of components:

### (1) `Card(clicked: Function, transparent: Boolean): Widget`

A generic rounded card.

### (2) `Tab(super args, icon: Widget, title: Widget, text: Wiget): Card`

The rectangular cards seen in the recommendations area.

### (3) `Tile(super args, icon: Widget, title: Widget): Card`

The square cards

### (4) `Text(type: String, text: String): Widget`

Different text sizes.

### (5) `Plural(children: Widget..., separator: Widget): Widget`

Used to contain several widgets. Takes an optional separator widget 

### (6) `Modal(child: Widget): Widget`

Used handle slide-up modals.
