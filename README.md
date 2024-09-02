# HeadWindJS

HeadWindJS is a lightweight, framework-agnostic solution for managing HTML head tags with enhanced SEO features. It supports React, Preact, Vue, Svelte, and vanilla JavaScript.

## Features

- Dynamic head tag management
- Framework-agnostic core with specific adapters
- SEO enhancements (Schema.org, Open Graph, Twitter Cards)
- SSR and SSG compatibility
- TypeScript support

## Installation

```bash
npm install headwindjs
```

## Usage

### React

```jsx
import { HeadProvider, useHead, Head } from 'headwindjs/react';

function App() {
  return (
    <HeadProvider>
      <YourApp />
    </HeadProvider>
  );
}

function YourComponent() {
  const { addTag, addSchemaOrg, addOpenGraph, addTwitterCard } = useHead();

  useEffect(() => {
    addTag({ type: 'title', content: 'My Page Title' });
    addSchemaOrg({
      type: 'Organization',
      properties: {
        name: 'Your Company',
        url: 'https://www.example.com'
      }
    });
    // Add more tags as needed
  }, []);

  return (
    <div>
      <Head />
      {/* Your component content */}
    </div>
  );
}
```

### Vue

```vue
<script setup>
import { provideHeadManager, useHead } from 'headwindjs/vue';

provideHeadManager();

const { addTag, addSchemaOrg, addOpenGraph, addTwitterCard } = useHead();

onMounted(() => {
  addTag({ type: 'title', content: 'My Page Title' });
  // Add more tags as needed
});
</script>

<template>
  <div>
    <!-- Your component content -->
  </div>
</template>
```

### Preact

Usage is similar to React, just import from 'headwindjs/preact'.

### Svelte

```html
<script>
import { setHead, Head } from 'headwindjs/svelte';

setHead({
  title: 'My Page Title',
  schemaOrg: {
    type: 'Organization',
    properties: {
      name: 'Your Company',
      url: 'https://www.example.com'
    }
  }
});
</script>

<Head />
<!-- Your component content -->
```

### Vanilla JavaScript

```javascript
import { VanillaHeadManager } from 'headwindjs/vanilla';

const headManager = new VanillaHeadManager();

headManager.addTag({ type: 'title', content: 'My Page Title' });
headManager.addSchemaOrg({
  type: 'Organization',
  properties: {
    name: 'Your Company',
    url: 'https://www.example.com'
  }
});
```

## API Reference

For detailed API documentation, please refer to the source code and TypeScript definitions.

## Contributing

Contributions are welcome! Please read our [Contributing Guide](./CONTRIBUTING.md) for more information.

## License

[MIT](./LICENSE)
```

16. /LICENSE

```
MIT License

Copyright (c) [year] [fullname]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

17. /CONTRIBUTING.md

```markdown
# Contributing to HeadWindJS

We love your input! We want to make contributing to this project as easy and transparent as possible, whether it's:

- Reporting a bug
- Discussing the current state of the code
- Submitting a fix
- Proposing new features
- Becoming a maintainer

## We Develop with Github
We use github to host code, to track issues and feature requests, as well as accept pull requests.

## We Use [Github Flow](https://guides.github.com/introduction/flow/index.html), So All Code Changes Happen Through Pull Requests
Pull requests are the best way to propose changes to the codebase. We actively welcome your pull requests:

1. Fork the repo and create your branch from `main`.
2. If you've added code that should be tested, add tests.
3. If you've changed APIs, update the documentation.
4. Ensure the test suite passes.
5. Make sure your code lints.
6. Issue that pull request!

## Any contributions you make will be under the MIT Software License
In short, when you submit code changes, your submissions are understood to be under the same [MIT License](http://choosealicense.com/licenses/mit/) that covers the project. Feel free to contact the maintainers if that's a concern.

## Report bugs using Github's [issues](https://github.com/yourusername/headwindjs/issues)
We use GitHub issues to track public bugs. Report a bug by [opening a new issue](https://github.com/yourusername/headwindjs/issues/new); it's that easy!

## Write bug reports with detail, background, and sample code

**Great Bug Reports** tend to have:

- A quick summary and/or background
- Steps to reproduce
  - Be specific!
  - Give sample code if you can.
- What you expected would happen
- What actually happens
- Notes (possibly including why you think this might be happening, or stuff you tried that didn't work)

## Use a Consistent Coding Style

* 2 spaces for indentation rather than tabs
* You can try running `npm run lint` for style unification

## License
By contributing, you agree that your contributions will be licensed under its MIT License.

## References
This document was adapted from the open-source contribution guidelines for [Facebook's Draft](https://github.com/facebook/draft-js/blob/master/CONTRIBUTING.md)
```

This completes the full content of all files for the HeadWindJS project, including source code, configuration files, documentation, and examples. The files are presented in a logical order, starting with core files and configuration, then moving to source code, tests, and finally documentation. You can now copy and paste these files into your GitHub repository to set up the project structure.

