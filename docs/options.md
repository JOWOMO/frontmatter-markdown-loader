# Options

The loader accepts some options to configure.

[[toc]]

## Mode

`options.mode` option requests what you can import from frontmatter markdown data.

This option is added since 2.0.0. So see [Migration Guide](migration) if you are using `1.x`.

```js
import Mode from 'frontmatter-markdown-loader/mode'
```

will provide constats. `Mode.NAME_OF_MODE`.

::: tip Plain string is available
You may not want to import `frontmatter-markdown-loader/mode`. Then you can just give string instead.

- `Mode.HTML` -> `"html"`
- `Mode.BODY` -> `"body"`
- `Mode.META` -> `"meta"`
- `Mode.VUE_COMPONENT` -> `"vue-component"`
- `Mode.VUE_RENDER_FUNCTIONS` -> `"vue-render-functions"`
- `Mode.REACT` -> `"react-component"`
:::

### Default

By default (When we don't specify any `mode` option), `attributes` and `html` are only available.

That's equivalent to:

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    mode: [Mode.HTML]
  }
}
```

```js
import fm from "something.md"

fm.attributes //=> { subject: "Hello", tags: ["tag1", "tag2"] }
fm.html //=> "<h1>Title</h1>\n<p>message</p>\n"
```

### Raw markdown

`Mode.BODY` gives raw string of Markdown part on frontmatter markdown.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    mode: [Mode.BODY]
  }
}
```

```js
import fm from "something.md"

fm.attributes //=> { subject: "Hello", tags: ["tag1", "tag2"] }
fm.html //=> undefined
fm.body //=> "# Title\n\nmessage\n"
```

### Metadata

`Mode.META` shows the metadata of the frontmatter markdown file.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    mode: [Mode.META]
  }
}
```

```js
import fm from "something.md"

fm.meta //=> { resourcePath: "/somepath/something.md" }
```

Currently, only `resourcePath` is available which returns [the path for the file in Webpack's context](https://webpack.js.org/api/loaders/#thisresourcepath).

### Vue component

`Mode.VUE_COMPONENT` requests to get the extendable component object of Vue.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    mode: [Mode.VUE_COMPONENT]
  }
}
```

```js
import fm from "something.md"

fm.vue.component //=> The object which can be extendable as Vue component which has compiled HTML as the template
```

::: tip How to use in Vue
To see the usage of `fm.vue.component`, see [this page](/vue).
:::

### Vue's render functions

`Mode.VUE_RENDER_FUNCTIONS` requests to get functions which are required to build Vue component. The function is having the compiled markdown as template of component.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    mode: [Mode.VUE_RENDER_FUNCTIONS]
  }
}
```

```js
import fm from "something.md"

fm.vue.render //=> render function as string
fm.vue.staticRenderFns //=> List of staticRender function as string
```

::: tip How to use in Vue
To see the usage of `fm.vue.component`, see [this page](/vue).
:::

### React component

`Mode.VUE_COMPONENT` requests to get the extendable component object of Vue.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    mode: [Mode.REACT]
  }
}
```

```js
import fm from "something.md"

fm.react //=> The function which is renderable as React component which has compiled markdown as template
```

::: tip How to use in React
To see the usage of `fm.react`, see [this page](/react).
:::

## Markdown compilation

### Configure markdown-it

By default, the loader compiles markdown with [markdown-it](https://github.com/markdown-it/markdown-it) package. `markdownIt` option accepts the configuration to overwrite the default.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    markdownIt: {
      html: true,
      linkify: true,
      breaks: true
    }
  }
}
```

[Refer markdown-it document for the further about the configuration](https://github.com/markdown-it/markdown-it#init-with-presets-and-options).

If `markdownIt` option isn't given, the loader uses `markdown-it` with just `{ html: true}` as default.

`markdonIt` option also accepts the instance of a markdown-it rederer (with [plugins](https://www.npmjs.com/search?q=keywords:markdown-it-plugin), for instance):

```js
const markdownIt = require('markdown-it');
const markdownItPrism = require('markdown-it-prism');
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    markdownIt: markdownIt({ html: true }).use(markdownItPrism)
  }
}
```

### Custom compiler

To provide the custom compilation logic, `markdown` option accepts the callback function which takes the string of the markdown source for its argument. And expects the function returns compiled HTML.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    markdown: (body) => {
      return compileWithCustomCompiler(body)
    }
  }
}
```

## Vue's root element

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    vue: {
      root: 'dynamicContent'
    }
  }
}
```

can specify the class name of the root element on the imported Vue's template.
