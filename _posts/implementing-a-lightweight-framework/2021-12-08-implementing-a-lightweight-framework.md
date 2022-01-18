# 6. Implementing A Lightweight Framework

After discovering a simple way to achieve native reactivity, we set out to write a tiny framework implementing the technique. When we say tiny, we mean *really* small — the framework in minified form is only about 180 bytes. That’s 90 less characters than this paragraph.

It’s also implemented efficiently. The time taken for an observable to update a subscriber is linear in the number of subscribers the observable has. In most cases that number is no more than 2-3. Hence, the view updates very quickly. Overall there is negligible overhead (and a degree of time saving) caused by using the framework.

## Example

Here’s a simple reactive text element. It shows the X coordinate of the user’s mouse. The function `when()` creates an observable variable that’s bound so some event listener.

```jsx
import { when } from '/lib/indigo.js'
import Text from '/components/Text.js'

function App() {
		const mouseX = when('mousemove', e => e.clientX)

    return Text(mouseX)
}
```

[https://streamable.com/ddedua](https://streamable.com/ddedua)

## Using `shadow()`

We created a function called `shadow()` that adds proper encapsulation to components via `ShadowRoot`. This allows components to use private styles for example.

```jsx

function Grid(columns, ...children) {

		return widget(
				css`
						:host {
								display: grid;
								grid-template-columns: repeat(1fr, ${columns});
						}
				`,
				...children
		)
}

function Card(title, body) {

		return widget(
				css`
						:host {
								border-radius: 4px;
								padding: 2rem;
						}
		
						h1 {
								font-size: 2rem;
								color: #222;
						}
				`,
				Heading(1, title),
				Paragraph(body)
		)
}

function App() {

		return Grid(4,
				Card('Hello', 'lorem ipsum'),
				Card('World', 'ipsum lorem'),
				Card('Foo', 'foo foo foo foo'),
				Card('Bar', 'bar bar bar bar')
		)
}
```