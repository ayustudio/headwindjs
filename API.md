# HeadWindJS API Documentation

This document provides a detailed overview of the HeadWindJS API for each supported framework.

## Core API

The core `HeadManager` class is used internally by all framework-specific adapters. It provides the following methods:

- `addTag(tag: Tag): void`
- `removeTag(tag: Tag): void`
- `getTags(): Tag[]`
- `renderToString(): string`
- `addSchemaOrg(data: Record<string, any>): void`
- `addOpenGraph(data: Record<string, string>): void`
- `addTwitterCard(data: Record<string, string>): void`

Where `Tag` is defined as:

```typescript
type TagType = 'title' | 'meta' | 'link' | 'script';

interface Tag {
  type: TagType;
  attributes: Record<string, string>;
  content?: string;
}
```

## React API

### HeadProvider

A component that provides the HeadManager context to its children.

```jsx
import { HeadProvider } from 'headwindjs/react';

function App() {
  return (
    <HeadProvider>
      {/* Your app components */}
    </HeadProvider>
  );
}
```

### useHead

A hook that provides access to the HeadManager methods.

```jsx
import { useHead } from 'headwindjs/react';

function YourComponent() {
  const { addTag, removeTag, addSchemaOrg, addOpenGraph, addTwitterCard } = useHead();

  // Use these methods to manage head tags
}
```

### Head

A component that renders the current head tags.

```jsx
import { Head } from 'headwindjs/react';

function YourComponent() {
  return (
    <>
      <Head />
      {/* Your component content */}
    </>
  );
}
```

## Preact API

The Preact API is identical to the React API, but imported from 'headwindjs/preact'.

## Vue API

### provideHeadManager

A function that provides the HeadManager to the Vue app.

```vue
<script setup>
import { provideHeadManager } from 'headwindjs/vue';

provideHeadManager();
</script>
```

### useHead

A composable that provides access to the HeadManager methods.

```vue
<script setup>
import { useHead } from 'headwindjs/vue';

const { addTag, removeTag, addSchemaOrg, addOpenGraph, addTwitterCard, tags } = useHead();

// Use these methods to manage head tags
</script>
```

### HeadManagerPlugin

A Vue plugin that automatically provides the HeadManager to the entire app.

```javascript
import { createApp } from 'vue';
import { HeadManagerPlugin } from 'headwindjs/vue';
import App from './App.vue';

const app = createApp(App);
app.use(HeadManagerPlugin);
app.mount('#app');
```

## Svelte API

### setHead

A function to set head tags.

```javascript
import { setHead } from 'headwindjs/svelte';

setHead({
  title: 'Page Title',
  meta: { description: 'Page description' },
  link: { canonical: 'https://example.com' },
  script: { content: 'console.log("Hello, world!")' },
  schemaOrg: { /* schema.org data */ },
  openGraph: { /* Open Graph data */ },
  twitterCard: { /* Twitter Card data */ }
});
```

### Head

A component that renders the current head tags.

```svelte
<script>
import { Head } from 'headwindjs/svelte';
</script>

<Head />
```

### tagsStore

A Svelte store containing the current head tags.

```javascript
import { tagsStore } from 'headwindjs/svelte';

tagsStore.subscribe(tags => {
  console.log('Current head tags:', tags);
});
```

## Vanilla JavaScript API

### VanillaHeadManager

A class that extends the core HeadManager with methods to update the DOM.

```javascript
import { VanillaHeadManager } from 'headwindjs/vanilla';

const headManager = new VanillaHeadManager();

headManager.addTag({ type: 'title', content: 'Page Title' });
headManager.addSchemaOrg({ /* schema.org data */ });
headManager.addOpenGraph({ /* Open Graph data */ });
headManager.addTwitterCard({ /* Twitter Card data */ });
```

## Common Methods Across All Frameworks

### addTag(tag: Tag)

Adds a new tag to the head.

```javascript
addTag({ type: 'meta', attributes: { name: 'description', content: 'Page description' } });
```

### removeTag(tag: Tag)

Removes a tag from the head.

```javascript
removeTag({ type: 'meta', attributes: { name: 'description' } });
```

### addSchemaOrg(data: Record<string, any>)

Adds schema.org structured data.

```javascript
addSchemaOrg({
  '@type': 'Organization',
  name: 'Your Company',
  url: 'https://example.com'
});
```

### addOpenGraph(data: Record<string, string>)

Adds Open Graph meta tags.

```javascript
addOpenGraph({
  title: 'Page Title',
  description: 'Page description',
  image: 'https://example.com/image.jpg',
  url: 'https://example.com'
});
```

### addTwitterCard(data: Record<string, string>)

Adds Twitter Card meta tags.

```javascript
addTwitterCard({
  card: 'summary_large_image',
  site: '@yourTwitterHandle',
  title: 'Page Title',
  description: 'Page description',
  image: 'https://example.com/image.jpg'
});
```

This API documentation provides a comprehensive overview of HeadWindJS's features across all supported frameworks. Users can refer to this document for detailed information on how to use the library in their projects.
```

You can add this `API.md` file to your project's root directory. It provides a detailed reference for users of your library across all supported frameworks. You may want to link to this file from your main README.md for easy access to the API documentation.
