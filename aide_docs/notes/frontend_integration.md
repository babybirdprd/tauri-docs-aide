# Frontend Integration

## Core Concepts
1. Requirements
   - Static hosting
   - Client-side rendering
   - No SSR support
   - Mobile dev server

2. Architecture Types
   - Static Site Gen (SSG)
   - Single Page Apps (SPA)
   - Multi Page Apps (MPA)
   - Client-server model

## Framework Support
1. JavaScript/TypeScript
   - Vite (recommended)
   - Next.js
   - Nuxt
   - SvelteKit
   - Qwik

2. Rust Frameworks
   - Leptos
   - Trunk
   - WASM support
   - Native integration

## Configuration
1. Development
```json
{
  "build": {
	"devPath": "http://localhost:3000",
	"beforeDevCommand": "npm run dev"
  }
}
```

2. Production
   - Static output
   - Asset bundling
   - Build optimization
   - Path configuration

## Best Practices
1. Setup
   - Framework choice
   - Build config
   - Dev server
   - Asset handling

2. Integration
   - API patterns
   - Static assets
   - Build process
   - Mobile support