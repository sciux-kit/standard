# Sciux Standard Syntax 1.0 Preview

> Version: `1.0-preview`
> 
> Date: `2025-06-27`
>
> Author: `Sciux Community <https://sciux.dev>`

Sciux is a markup language for creating interactive document with XML-like syntax, which is designed to the generation of user-friendly graphs of the LLM (Large Language Model)

Sciux is a standard syntax, not a renderer, exclude any components, animations and function tools.

## Structure

### Node

#### Text Node

The most basic sciux node is the text node.

```
Hello world!
```

#### Element Node

A element node has three elements: `tag`, `attributes` ,`children`.

- `tag`
    - `<name></name>` A common tag should use `<xxx>` to start and `</xxx>` to end up.
    - `<name/>` A self-closing tag should use `<xxx/>` to define
    - A single `<xxx>` is not allowed.
- `attributes`
    - A attribute should use `key="value"` to define, and write in the start symbol: `<name attr="xxx"/>`
    - Mutiple attributes should be divided by space (` `): `<name attr1="xxx" attr2="xxx"/>`
    - The attribute name start with `:` is a JavaScript expression: `<name :attr="1 + 2"/>`
    - The attribute name start with `#` is a statement
    - The attribute name start with `@` is an event with JavaScript handler: `<name @click="x++"/>`
    - The attribute name start with `$` is an animation
- `children`
    - Children should be in the range from start symbol to end symbol: `<name>children</name>`
    - A child could be a text node, element node or value node
    - self-closing tag is not allowed to have children nodes

#### Value Node

A value node should start with `{{` and end with `}}`

The value is a JavaScript expression, it could be string, boolean, number, object or more data type, but the JavaScript statement is not supported.

```
{{ 1 + 2 }}
```

### Statement

#### for

Statement `#for` can be used to iterate over a list of data.

```
<element #for="i in [1, 2, 3]">{{ i }}</element>
```

value structure: `[name] in [value]`
- `[name]` could use in the children of the node's everywhere supported JavaScript expression
- `[value]` should be iterable in JavaScript Standard

#### if

Statement `#if` can be used to conditionally render a node.

```
<element #if="x > 10">{{ x }}</element>
```

value structure: `[condition]`
- `[condition]` should be a JavaScript expression, and the result should be a boolean value

#### elif

Statement `#elif` can be used to conditionally render a node.

```
<element #if="x < 10">{{ x + 1 }}<element/>
<element #elif="x > 10">{{ x }}</element>
```

- `[condition]` should be a JavaScript expression, and the result should be a boolean value
- `#elif` should be used after `#if` or `#elif`

#### else

Statement `#else` can be used to conditionally render a node.

```
<element #if="x < 10">{{ x + 1 }}<element/>
<element #else>{{ x }}</element>
```

- `#else` should be used after `#if` or `#elif`

### Event

State `@event` can be used to define an event handler.

```
<element @click="clickHandler">{{ x }}</element>
```

value structure: `[event]`
- `[event]` should be a JavaScript expression, and the result should be a boolean value
- Event type supported: All the event type in JavaScript Standard

### Animation

State `$animation` can be used to define an animation.

```
<element $="">{{ x }}</element>
```

value structure: `[animation]`
- `[name],[duration],[easing]?`: A animation `[name]` excute complete in `[duration]` with `[easing]`, the default `[easing]` is linear (JavaScript expression: `(x) => x`)
- `[name],[duration] [name],[duration]`: Mutiple animation should be separated by space (` `), this animation will be executed in order
- `parallel([name],[duration] [name],[duration])`: Parallel animation should be contained in the `parallel([aniamtions])`, this animation will be executed in parallel
- Using JavaScript expression in `[duration]` is not supported
- A `[easing]` is a one-to-result function, with a number input and a number output

key structure: `$[event]?`
- A `$` without any other text means the animation will be execute immediately
- `$[event]` means the animation will be execute when the event is triggered

### Reactive

A the nodes are reactive, which means it will be updated when the data is changed.

You can use a special built-in element node `<let>` to define reactive data:

```
<let :x="10" :y="20"/>
```

And you can use this variable in everywhere supported JavaScript expression.

```
<let :x="10" :y="20"/>
<element :attr1="x" :attr2="y" :attr3="x - y">{{ x + y }}</element>
```

> This documentation is just for a preview, and the final syntax is not determined.
