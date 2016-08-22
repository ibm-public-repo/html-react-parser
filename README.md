# html-react-parser

An HTML to React parser.

```
Parser(htmlString[, options])
```

The parser converts an HTML string to [React element(s)](https://facebook.github.io/react/docs/glossary.html#react-elements). You can also `replace` element(s) with your own custom React element(s) via the parser options.

### Example

```js
var Parser = require('html-react-parser');
var reactElement = (
    Parser('<p>Hello, world!</p>') // equivalent to `React.createElement('p', {}, 'Hello, world!')`
);
ReactDOM.render(reactElement, document.getElementById('node'));
```

## Installation

```sh
$ npm install html-react-parser
```

## Usage

Render to DOM:

```js
var Parser = require('html-react-parser');
var ReactDOM = require('react-dom');

// single element
ReactDOM.render(
    Parser('<p>single</p>'),
    document.getElementById('root')
);

// adjacent elements
ReactDOM.render(
    // the parser returns an array for adjacent elements
    // so make sure they are nested under a parent React element
    React.createElement('div', {}, Parser('<p>one</p><p>two</p>'))
    document.getElementById('root')
);

// nested elements
ReactDOM.render(
    Parser('<ul><li>inside</li></ul>'),
    document.getElementedById('root')
);

// attributes are preserved
ReactDOM.render(
    Parser('<section id="foo" class="bar baz" data-qux="42">look at me now</section>'),
    document.getElementById('root')
);
```

### Options

#### replace(domNode)

`replace` allows you to swap an element with your own React element.

The output of `domNode` is the same as the output from [htmlparser2.parseDOM](https://github.com/fb55/domhandler#example).

```js
var Parser = require('html-react-parser');
var React = require('react');

var html = '<div><p id="main">replace me</p></div>';

var reactElement = Parser(html, {
    replace: function(domNode) {
        // example `domNode`:
        // {  type: 'tag',
        //    name: 'p',
        //    attribs: { id: 'main' },
        //    children: [],
        //    next: null,
        //    prev: null,
        //    parent: [Circular] }
        if (domNode.attribs && domNode.attribs.id === 'main') {
            // element is replaced only if a valid React element is returned
            return React.createElement('span', { style: { fontSize: '42px' } }, 'replaced!');
        }
    }
});

var ReactDOM = require('react-dom');
ReactDOM.render(reactElement, document.getElementById('root'));
// <div><span style="font-size: 42px;">replaced!</span></div>
```

## Testing

```sh
$ npm test
```

## Special Thanks

To [benox3](https://github.com/benox3) and [tdlm](https://github.com/tdlm) for their feedback and review.

## License

[MIT](https://github.com/remarkablemark/html-react-parser/blob/master/LICENSE)
