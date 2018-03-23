# frontmatter-markdown-loader

[![npm](https://img.shields.io/npm/v/frontmatter-markdown-loader.svg?style=for-the-badge)](https://www.npmjs.com/package/frontmatter-markdown-loader)

Webpack Loader for: FrontMatter (.md) -> Markdown + Meta -> HTML + Meta (+ Vue template)

## Instllation

```
$ npm i -D frontmatter-markdown-loader
```

Or

```
$ yarn add -D frontmatter-markdown-loader
```

## Configuration

Configure the loader for Markdown files like:

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
}
```

Then you can get frontmatter attributes and compiled markdown.

```js
import fm from "something.md"

fm.attributes //=> FrontMatter attributes
fm.body //=> Markdown source
fm.html //=> Compiled Html
```

## Options

### Use your own markdown compiler

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    markdown: (body) => {
      return compileWithYourMDCompiler(body)
    }
  }
}
```

As default, compiling markdown body with [markdown-it](https://www.npmjs.com/package/markdown-it) with allowing HTML. So behave as same as:

```js
const md = require('markdown-it')

...

{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    markdown: (body) => {
      return md.render(body)
    }
  }
}
```


### Vue template

The loader could compile HTML section of files as Vue template.

```js
{
  test: /\.md$/,
  loader: 'frontmatter-markdown-loader'
  options: {
    vue: true
  }
}
```

This returns functions by compiled template as string `render`, `staticRenderFns` which are Vue component requires.

```js
import fm from "something.md"

fm.vue.render //=> render function as string
fm.vue.staticRenderFns //=> List of staticRender function as string
```

so, you can use them in your Vue component:

```js
import OtherComponent from "OtherComponent.vue"

export default {
  data: {
    templateRender: null
  },
  
  components: {
    OtherComponent // If markdown has `<other-component>` in body, will work :)
  }

  render: function (createElement) {
    return this.templateRender ? this.templateRender() : createElement("div", "Rendering");
  },

  created: function () {
    this.templateRender = new Function(fm.vue.render)();
    this.$options.staticRenderFns = new Function(fm.vue.staticRenderFns)();
  },
}
```

This component renders

## Inspired/Refered

- [egoist/vmark: Convert markdown to Vue component.](https://github.com/egoist/vmark)
- [webpack-contrib/json-loader: json loader module for webpack](https://github.com/webpack-contrib/json-loader)

## License

- [MIT](LICENSE) Copyright Kengo Hamasaki
