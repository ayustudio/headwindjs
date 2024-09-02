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

