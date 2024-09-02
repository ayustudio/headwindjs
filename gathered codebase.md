Certainly! I'll provide the complete content of all files for the HeadWindJS project, organized as requested. Each file will be preceded by its file path, and I'll include placeholder content for any files that haven't been fully implemented yet.

1. /package.json

```json
{
  "name": "headwindjs",
  "version": "1.0.0",
  "description": "A lightweight, framework-agnostic solution for managing HTML head tags with enhanced SEO features",
  "main": "dist/index.js",
  "module": "dist/index.esm.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "rollup -c",
    "test": "jest",
    "lint": "eslint 'src/**/*.{js,ts,tsx}'",
    "lint:fix": "eslint 'src/**/*.{js,ts,tsx}' --fix",
    "prepublishOnly": "npm run build"
  },
  "keywords": [
    "head",
    "manager",
    "seo",
    "react",
    "preact",
    "vue",
    "svelte",
    "vanilla"
  ],
  "author": "Your Name",
  "license": "MIT",
  "devDependencies": {
    "@rollup/plugin-typescript": "^8.3.0",
    "@types/jest": "^27.0.3",
    "@types/react": "^17.0.37",
    "jest": "^27.4.5",
    "preact": "^10.5.15",
    "react": "^17.0.2",
    "rollup": "^2.61.1",
    "rollup-plugin-terser": "^7.0.2",
    "svelte": "^3.44.0",
    "ts-jest": "^27.1.2",
    "tslib": "^2.3.1",
    "typescript": "^4.5.4",
    "vue": "^3.2.26",
    "@typescript-eslint/parser": "^4.33.0",
    "@typescript-eslint/eslint-plugin": "^4.33.0",
    "eslint": "^7.32.0",
    "eslint-plugin-react": "^7.26.1",
    "eslint-plugin-react-hooks": "^4.2.0"
  },
  "peerDependencies": {
    "react": "^16.8.0 || ^17.0.0",
    "preact": "^10.0.0",
    "vue": "^3.0.0",
    "svelte": "^3.0.0"
  },
  "files": [
    "dist"
  ]
}
```

2. /tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es2015",
    "module": "esnext",
    "lib": ["dom", "esnext"],
    "importHelpers": true,
    "declaration": true,
    "sourceMap": true,
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "moduleResolution": "node",
    "jsx": "react",
    "esModuleInterop": true,
    "outDir": "dist"
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

3. /rollup.config.js

```javascript
import typescript from '@rollup/plugin-typescript';
import { terser } from 'rollup-plugin-terser';
import pkg from './package.json';

const input = 'src/index.ts';

const external = [
  ...Object.keys(pkg.dependencies || {}),
  ...Object.keys(pkg.peerDependencies || {}),
  'react',
  'preact',
  'vue',
  'svelte'
];

const plugins = [
  typescript({ tsconfig: './tsconfig.json' }),
  terser()
];

export default [
  // CommonJS (for Node) and ES module (for bundlers) build
  {
    input,
    external,
    output: [
      { file: pkg.main, format: 'cjs', sourcemap: true },
      { file: pkg.module, format: 'es', sourcemap: true }
    ],
    plugins
  },

  // UMD build (for browsers)
  {
    input,
    output: {
      name: 'HeadWindJS',
      file: 'dist/headwindjs.umd.js',
      format: 'umd',
      globals: {
        react: 'React',
        preact: 'preact',
        vue: 'Vue',
        svelte: 'svelte'
      },
      sourcemap: true
    },
    external: ['react', 'preact', 'vue', 'svelte'],
    plugins
  }
];
```

4. /.eslintrc.js

```javascript
module.exports = {
  parser: '@typescript-eslint/parser',
  extends: [
    'plugin:@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
  ],
  parserOptions: {
    ecmaVersion: 2020,
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true,
    },
  },
  rules: {
    // Place to specify ESLint rules. Can be used to overwrite rules specified from the extended configs
    // e.g. "@typescript-eslint/explicit-function-return-type": "off",
  },
  settings: {
    react: {
      version: 'detect',
    },
  },
};
```

5. /.gitignore

```
# Dependency directories
node_modules/

# Build outputs
dist/

# Logs
*.log
npm-debug.log*

# Operating System Files
.DS_Store
Thumbs.db

# IDE Files
.vscode/
.idea/

# Test coverage
coverage/

# Temporary files
*.tmp
*.swp

# ESLint cache
.eslintcache
```

6. /src/core/HeadManager.ts

```typescript
export type TagType = 'title' | 'meta' | 'link' | 'script';

export interface Tag {
  type: TagType;
  attributes: Record<string, string>;
  content?: string;
}

export class HeadManager {
  private tags: Tag[] = [];

  addTag(tag: Tag): void {
    const index = this.findExistingTagIndex(tag);
    if (index !== -1) {
      this.tags[index] = tag;
    } else {
      this.tags.push(tag);
    }
  }

  removeTag(tag: Tag): void {
    const index = this.findExistingTagIndex(tag);
    if (index !== -1) {
      this.tags.splice(index, 1);
    }
  }

  getTags(): Tag[] {
    return this.tags;
  }

  renderToString(): string {
    return this.tags.map(tag => this.tagToString(tag)).join('\n');
  }

  addSchemaOrg(data: Record<string, any>): void {
    this.addTag({
      type: 'script',
      attributes: { type: 'application/ld+json' },
      content: JSON.stringify(data)
    });
  }

  addOpenGraph(data: Record<string, string>): void {
    Object.entries(data).forEach(([key, value]) => {
      this.addTag({
        type: 'meta',
        attributes: { property: `og:${key}`, content: value }
      });
    });
  }

  addTwitterCard(data: Record<string, string>): void {
    Object.entries(data).forEach(([key, value]) => {
      this.addTag({
        type: 'meta',
        attributes: { name: `twitter:${key}`, content: value }
      });
    });
  }

  private findExistingTagIndex(tag: Tag): number {
    return this.tags.findIndex(existingTag => 
      existingTag.type === tag.type &&
      JSON.stringify(existingTag.attributes) === JSON.stringify(tag.attributes)
    );
  }

  private tagToString(tag: Tag): string {
    const attributes = Object.entries(tag.attributes)
      .map(([key, value]) => `${key}="${value}"`)
      .join(' ');

    if (tag.type === 'meta' || tag.type === 'link') {
      return `<${tag.type} ${attributes}>`;
    } else {
      return `<${tag.type} ${attributes}>${tag.content || ''}</${tag.type}>`;
    }
  }
}
```

7. /src/react/HeadManager.tsx

```typescript
import React, { createContext, useContext, useEffect, useState } from 'react';
import { HeadManager, Tag } from '../core/HeadManager';

const HeadManagerContext = createContext<HeadManager | null>(null);

export const HeadProvider: React.FC = ({ children }) => {
  const [headManager] = useState(() => new HeadManager());

  return (
    <HeadManagerContext.Provider value={headManager}>
      {children}
    </HeadManagerContext.Provider>
  );
};

export const useHead = () => {
  const headManager = useContext(HeadManagerContext);
  if (!headManager) {
    throw new Error('useHead must be used within a HeadProvider');
  }

  const [, forceUpdate] = useState({});

  useEffect(() => {
    return () => {
      forceUpdate({});
    };
  }, []);

  return {
    addTag: headManager.addTag.bind(headManager),
    removeTag: headManager.removeTag.bind(headManager),
    addSchemaOrg: headManager.addSchemaOrg.bind(headManager),
    addOpenGraph: headManager.addOpenGraph.bind(headManager),
    addTwitterCard: headManager.addTwitterCard.bind(headManager),
  };
};

export const Head: React.FC = () => {
  const headManager = useContext(HeadManagerContext);
  if (!headManager) {
    throw new Error('Head must be used within a HeadProvider');
  }

  const tags = headManager.getTags();

  return (
    <>
      {tags.map((tag: Tag, index: number) => {
        if (tag.type === 'title') {
          return <title key={index}>{tag.content}</title>;
        }
        if (tag.type === 'meta') {
          return <meta key={index} {...tag.attributes} />;
        }
        if (tag.type === 'link') {
          return <link key={index} {...tag.attributes} />;
        }
        if (tag.type === 'script') {
          return <script key={index} {...tag.attributes}>{tag.content}</script>;
        }
        return null;
      })}
    </>
  );
};
```

8. /src/preact/HeadManager.tsx

```typescript
import { createContext, useContext, useEffect, useState } from 'preact/hooks';
import { FunctionComponent } from 'preact';
import { HeadManager, Tag } from '../core/HeadManager';

const HeadManagerContext = createContext<HeadManager | null>(null);

export const HeadProvider: FunctionComponent = ({ children }) => {
  const [headManager] = useState(() => new HeadManager());

  return (
    <HeadManagerContext.Provider value={headManager}>
      {children}
    </HeadManagerContext.Provider>
  );
};

export const useHead = () => {
  const headManager = useContext(HeadManagerContext);
  if (!headManager) {
    throw new Error('useHead must be used within a HeadProvider');
  }

  const [, forceUpdate] = useState({});

  useEffect(() => {
    return () => {
      forceUpdate({});
    };
  }, []);

  return {
    addTag: headManager.addTag.bind(headManager),
    removeTag: headManager.removeTag.bind(headManager),
    addSchemaOrg: headManager.addSchemaOrg.bind(headManager),
    addOpenGraph: headManager.addOpenGraph.bind(headManager),
    addTwitterCard: headManager.addTwitterCard.bind(headManager),
  };
};

export const Head: FunctionComponent = () => {
  const headManager = useContext(HeadManagerContext);
  if (!headManager) {
    throw new Error('Head must be used within a HeadProvider');
  }

  const tags = headManager.getTags();

  return (
    <>
      {tags.map((tag: Tag, index: number) => {
        if (tag.type === 'title') {
          return <title key={index}>{tag.content}</title>;
        }
        if (tag.type === 'meta') {
          return <meta key={index} {...tag.attributes} />;
        }
        if (tag.type === 'link') {
          return <link key={index} {...tag.attributes} />;
        }
        if (tag.type === 'script') {
          return <script key={index} {...tag.attributes}>{tag.content}</script>;
        }
        return null;
      })}
    </>
  );
};
```

9. /src/vue/HeadManager.ts

```typescript
import { inject, provide, reactive, onUnmounted } from 'vue';
import { HeadManager } from '../core/HeadManager';

const headManagerKey = Symbol('headManager');

export function provideHeadManager() {
  const headManager = new HeadManager();
  provide(headManagerKey, headManager);
  return headManager;
}

export function useHead() {
  const headManager = inject<HeadManager>(headManagerKey);
  if (!headManager) {
    throw new Error('useHead must be used within a component that called provideHeadManager');
  }

  const state = reactive({
    tags: headManager.getTags(),
  });

  const updateTags = () => {
    state.tags = headManager.getTags();
  };

  const addTag = (tag: Parameters<HeadManager['addTag']>[0]) => {
    headManager.addTag(tag);
    updateTags();
  };

  const removeTag = (tag: Parameters<HeadManager['removeTag']>[0]) => {
    headManager.removeTag(tag);
    updateTags();
  };

  onUnmounted(() => {
    updateTags();
  });

  return {
    addTag,
    removeTag,
    addSchemaOrg: headManager.addSchemaOrg.bind(headManager),
    addOpenGraph: headManager.addOpenGraph.bind(headManager),
    addTwitterCard: headManager.addTwitterCard.bind(headManager),
    tags: state.tags,
  };
}

export const HeadManagerPlugin = {
  install(app: any) {
    const headManager = provideHeadManager();
    app.config.globalProperties.$headManager = headManager;
  },
};
```

10. /src/svelte/HeadManager.ts

```typescript
import { writable } from 'svelte/store';
import { HeadManager, Tag } from '../core/HeadManager';

const headManager = new HeadManager();
const tagsStore = writable<Tag[]>([]);

function updateTags() {
  tagsStore.set(headManager.getTags());
}

export function setHead(options: {
  title?: string;
  meta?: Record<string, string>;
  link?: Record<string, string>;
  script?: { content: string; attributes?: Record<string, string> };
  schemaOrg?: Record<string, any>;
  openGraph?: Record<string, string>;
  twitterCard?: Record<string, string>;
}) {
  if (options.title) {
    headManager.addTag({ type: 'title', content: options.title, attributes: {} });
  }

  if (options.meta) {
    Object.entries(options.meta).forEach(([name, content]) => {
      headManager.addTag({ type: 'meta', attributes: { name, content } });
    });
  }

  if (options.link) {
    Object.entries(options.link).forEach(([rel, href]) => {
      headManager.addTag({ type: 'link', attributes: { rel, href } });
    });
  }

  if (options.script) {
    headManager.addTag({
      type: 'script',
      content: options.script.content,
      attributes: options.script.attributes || {}
    });
  }

  if (options.schemaOrg) {
    headManager.addSchemaOrg(options.schemaOrg);
  }

  if (options.openGraph) {
    headManager.addOpenGraph(options.openGraph);
  }

  if (options.twitterCard) {
    headManager.addTwitterCard(options.twitterCard);
  }

  updateTags();
}

export { tagsStore };

export function Head(target: HTMLElement) {
  let unsubscribe: () => void;

  function update(tags: Tag[]) {
    target.innerHTML = '';
    tags.forEach(tag => {
      const element = document.createElement(tag.type);
      Object.entries(tag.attributes).forEach(([key, value]) => {
        element.setAttribute(key, value);
      });
      if (tag.content) {
        element.textContent = tag.content;
      }
      target.appendChild(element);
    });
  }

  unsubscribe = tagsStore.subscribe(update);

  return {
    destroy() {
      unsubscribe();
    }
  };
}
```

11. /src/vanilla/HeadManager.ts

```typescript
import { HeadManager } from '../core/HeadManager';

export class VanillaHeadManager extends HeadManager {
  constructor() {
    super();
  }

  updateDOM(): void {
    const head = document.head;
    const oldTags = head.querySelectorAll('[data-headwindjs]');
    oldTags.forEach(tag => tag.remove());

    this.getTags().forEach(tag => {
      const element = document.createElement(tag.type);
      Object.entries(tag.attributes).forEach(([key, value]) => {
        element.setAttribute(key, value);
      });
      if (tag.content) {
        element.textContent = tag.content;
      }
      element.setAttribute('data-headwindjs', '');
      head.appendChild(element);
    });
  }

  addTag(tag: Parameters<HeadManager['addTag']>[0]): void {
    super.addTag(tag);
    this.updateDOM();
  }

  removeTag(tag: Parameters<HeadManager['removeTag']>[0]): void {
    super.removeTag(tag);
    this.updateDOM();
  }

  addSchemaOrg(data: Record<string, any>): void {
    super.addSchemaOrg(data);
    this.updateDOM();
  }

  addOpenGraph(data: Record<string, string>): void {
    super.addOpenGraph(data);
    this.updateDOM();
  }

  addTwitterCard(data: Record<string, string>): void {
    super.addTwitterCard(data);
    this.updateDOM();
  }
}
```

12. /src/index.ts

```typescript
export { HeadManager } from './core/HeadManager';
export * from './react/HeadManager';
export * from './preact/HeadManager';
export * from './vue/HeadManager';
export * from './svelte/HeadManager';
export { VanillaHeadManager } from './vanilla/HeadManager';
```

13. /tests/core.test.ts

```typescript
import { HeadManager } from '../src/core/HeadManager';

describe('HeadManager', () => {
  let headManager: HeadManager;

  beforeEach(() => {
    headManager = new HeadManager();
  });

  test('addTag adds a new tag', () => {
    headManager.addTag({ type: 'title', attributes: {}, content: 'Test Title' });
    expect(headManager.getTags()).toHaveLength(1);
    expect(headManager.getTags()[0]).toEqual({ type: 'title', attributes: {}, content: 'Test Title' });
  });

  test('removeTag removes an existing tag', () => {
    const tag = { type: 'meta' as const, attributes: { name: 'description', content: 'Test description' } };
    headManager.addTag(tag);
    headManager.removeTag(tag);
    expect(headManager.getTags()).toHaveLength(0);
  });

  test('addSchemaOrg adds schema.org data', () => {
    headManager.addSchemaOrg({ '@type': 'Organization', name: 'Test Org' });
    const tags = headManager.getTags();
    expect(tags).toHaveLength(1);
    expect(tags[0].type).toBe('script');
    expect(tags[0].attributes.type).toBe('application/ld+json');
    expect(JSON.parse(tags[0].content!)).toEqual({ '@type': 'Organization', name: 'Test Org' });
  });

  test('addOpenGraph adds Open Graph tags', () => {
    headManager.addOpenGraph({ title: 'OG Title', description: 'OG Description' });
    const tags = headManager.getTags();
    expect(tags).toHaveLength(2);
    expect(tags[0]).toEqual({ type: 'meta', attributes: { property: 'og:title', content: 'OG Title' } });
    expect(tags[1]).toEqual({ type: 'meta', attributes: { property: 'og:description', content: 'OG Description' } });
  });

  test('addTwitterCard adds Twitter Card tags', () => {
    headManager.addTwitterCard({ card: 'summary', title: 'Twitter Title' });
    const tags = headManager.getTags();
    expect(tags).toHaveLength(2);
    expect(tags[0]).toEqual({ type: 'meta', attributes: { name: 'twitter:card', content: 'summary' } });
    expect(tags[1]).toEqual({ type: 'meta', attributes: { name: 'twitter:title', content: 'Twitter Title' } });
  });

  test('renderToString returns correct HTML string', () => {
    headManager.addTag({ type: 'title', attributes: {}, content: 'Test Title' });
    headManager.addTag({ type: 'meta', attributes: { name: 'description', content: 'Test description' } });
    const html = headManager.renderToString();
    expect(html).toBe('<title>Test Title</title>\n<meta name="description" content="Test description">');
  });
});
```

14. /tests/react.test.tsx

```typescript
import React from 'react';
import { render, act } from '@testing-library/react';
import { HeadProvider, useHead, Head } from '../src/react/HeadManager';

describe('React HeadManager', () => {
  test('HeadProvider provides context', () => {
    const TestComponent = () => {
      const head = useHead();
      return head ? <div>Head manager provided</div> : <div>No head manager</div>;
    };

    const { getByText } = render(
      <HeadProvider>
        <TestComponent />
      </HeadProvider>
    );

    expect(getByText('Head manager provided')).toBeTruthy();
  });

  test('useHead hook adds tags', () => {
    const TestComponent = () => {
      const { addTag } = useHead();
      React.useEffect(() => {
        addTag({ type: 'title', attributes: {}, content: 'Test Title' });
      }, [addTag]);
      return <Head />;
    };

    const { container } = render(
      <HeadProvider>
        <TestComponent />
      </HeadProvider>
    );

    expect(container.querySelector('title')?.textContent).toBe('Test Title');
  });

  test('Head component renders tags', () => {
    const TestComponent = () => {
      const { addTag } = useHead();
      React.useEffect(() => {
        addTag({ type: 'meta', attributes: { name: 'description', content: 'Test description' } });
      }, [addTag]);
      return <Head />;
    };

    const { container } = render(
      <HeadProvider>
        <TestComponent />
      </HeadProvider>
    );

    const metaTag = container.querySelector('meta[name="description"]');
    expect(metaTag).toBeTruthy();
    expect(metaTag?.getAttribute('content')).toBe('Test description');
  });
});
