# 01 - Hello Vue CDN

## Overview
Your first Vue.js application! This project introduces the absolute basics of Vue.js using a CDN link - no build tools, no complex setup, just pure Vue fundamentals following the official Vue documentation.

## What You'll Learn
- How to include Vue.js via CDN using the global build
- Creating your first Vue application instance
- Basic template syntax and interpolation
- Understanding the Vue app mounting process
- Introduction to Vue's Composition API with `ref`

## Official Vue CDN Setup

### Including Vue from CDN
Vue can be used directly from a CDN via a script tag:
```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

You can also use other CDNs like **jsdelivr** or **cdnjs**, or download and serve the file yourself.

### Key Concepts

### Vue Global Build
When using the CDN, all top-level APIs are exposed as properties on the global `Vue` object:
```javascript
const { createApp, ref } = Vue
```

### Creating Your First App
Here's the complete structure following Vue's official documentation:
```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">{{ message }}</div>

<script>
  const { createApp, ref } = Vue

  createApp({
    setup() {
      const message = ref('Hello vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

### Template Interpolation
Use double curly braces `{{ }}` to display data in your HTML:
```html
<div id="app">
  <h1>{{ message }}</h1>
  <p>{{ greeting }}</p>
</div>
```

### Reactive References
Use `ref()` to create reactive data that automatically updates the template:
```javascript
const { createApp, ref } = Vue

createApp({
  setup() {
    const message = ref('Hello Vue!')
    const greeting = ref('Welcome to Vue.js')
    
    return {
      message,
      greeting
    }
  }
}).mount('#app')
```

## Code Structure
```
index.html          # Main HTML file with Vue CDN
├── Vue CDN link    # <script src="https://unpkg.com/vue@3/dist/vue.global.js">
├── App container   # <div id="app">
├── Template        # HTML with Vue interpolation syntax
└── Script          # Vue app creation with setup() function
```

## Understanding the Setup Function
The `setup()` function is where you define your component's reactive state and logic:
```javascript
setup() {
  // Create reactive data
  const message = ref('Hello Vue!')
  const count = ref(0)
  
  // Return data to make it available in template
  return {
    message,
    count
  }
}
```

## Important Notes
- The `#app` element is where Vue will mount and take control
- Everything inside `#app` becomes part of Vue's template
- `ref()` creates reactive references that trigger template updates
- You must return data from `setup()` to use it in templates
- When using CDN, there's no build step - perfect for learning basics

## Try This
1. Change the message text and see it update in the browser
2. Add new reactive data with `ref()` and display it
3. Try mathematical expressions in the template: `{{ 2 + 3 }}`
4. Use JavaScript expressions: `{{ message.toUpperCase() }}`
5. Create multiple reactive values and experiment with interpolation

## Example Expansions
```javascript
const { createApp, ref } = Vue

createApp({
  setup() {
    const message = ref('Hello Vue!')
    const name = ref('World')
    const count = ref(42)
    const isVisible = ref(true)
    
    return {
      message,
      name,
      count,
      isVisible
    }
  }
}).mount('#app')
```

```html
<div id="app">
  <h1>{{ message }}</h1>
  <p>Hello, {{ name }}!</p>
  <p>Count: {{ count }}</p>
  <p>Math: {{ count * 2 }}</p>
  <p>Conditional: {{ isVisible ? 'Visible' : 'Hidden' }}</p>
</div>
```

## Common Gotchas
- Make sure the CDN link loads before your Vue code
- The element you mount to (`#app`) should exist in the HTML
- Remember to return reactive data from the `setup()` function
- Use `ref()` for primitive values to make them reactive
- Vue 3 Composition API syntax is different from Vue 2

## Next Steps
Once you're comfortable with basic Vue mounting, reactive references, and interpolation, you're ready to explore more advanced reactivity and data binding in the next project!