<h1 align="center">Shiki Rehype</h1>
<h3 align="center"><a href="https://github.com/rehypejs/rehype">Rehype</a> plugin to highlight code blocks with <a href="https://shiki.matsu.io">Shiki</a></h3>
<p align="center">
<a href="https://github.com/leafac/shiki-rehype"><img src="https://img.shields.io/badge/Source---" alt="Source"></a>
<a href="https://www.npmjs.com/package/shiki-rehype"><img alt="Package" src="https://badge.fury.io/js/shiki-rehype.svg"></a>
<a href="https://github.com/leafac/shiki-rehype/actions"><img src="https://github.com/leafac/shiki-rehype/workflows/.github/workflows/main.yml/badge.svg" alt="Continuous Integration"></a>
</p>

### Installation

```console
$ npm install shiki-rehype
```

### Example

```js
const unified = require("unified");
const remarkParse = require("remark-parse");
const remarkRehype = require("remark-rehype");
const rehypeShiki = require("shiki-rehype");
const rehypeStringify = require("rehype-stringify");
const shiki = require("shiki");

(async () => {
  console.log(
    unified()
      .use(remarkParse)
      .use(remarkRehype)
      .use(rehypeShiki, {
        highlighter: await shiki.getHighlighter({ theme: "light-plus" }),
      })
      .use(rehypeStringify)
      .processSync(
        `
\`\`\`javascript
return unified()
\`\`\`
`
      )
      .toString()
  ); // => "<pre class=\\"shiki\\" style=\\"background-color: #FFFFFF\\"><code><span class=\\"line\\"><span style=\\"color: #AF00DB\\">return</span><span style=\\"color: #000000\\"> </span><span style=\\"color: #795E26\\">unified</span><span style=\\"color: #000000\\">()</span></span></code></pre>"
})();
```

### Options

- `highlighter` (required): An instance of the Shiki highlighter.
- `throwOnUnsupportedLanguage` (default: `false`): A boolean indicating whether to throw an exception if a code block refers to an unsupported language.

### Security

Shiki Rehype doesn’t open you up to [cross-site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks as long as Shiki doesn’t (which it doesn’t).

### How Is This Different from [rehype-shiki](https://github.com/rsclarke/rehype-shiki)?

1. TypeScript support.
2. Shiki is declared as a [`peerDependency`](https://docs.npmjs.com/cli/v6/configuring-npm/package-json#peerdependencies), so Shiki Rehype doesn’t have to be updated when new versions of Shiki are released (as long as Shiki’s API remain compatible). See https://github.com/rsclarke/rehype-shiki/pull/48 https://github.com/rsclarke/rehype-shiki/pull/46 https://github.com/rsclarke/rehype-shiki/issues/47 https://github.com/rsclarke/rehype-shiki/issues/2.
3. You must pass in an instance of the Shiki highlighter, Shiki Rehype won’t create one for you. This means that:
   1. The Shiki highlighter instance is reused on every invocation of the processor, [instead of being recreated every time you call the processor](https://github.com/rsclarke/rehype-shiki/blob/3ebaeab3297d1cbe9ac75e2294ab636bbe250541/index.js#L38-L43).
   2. The transformer is synchronous, so you may use it with `.processSync()`.

That said, I’ll contact the maintainers of rehype-shiki and try to merge the code bases. We’ll see…
