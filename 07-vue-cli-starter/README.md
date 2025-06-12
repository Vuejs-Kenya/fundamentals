# 07 - Your First Real Vue Project Setup! 🚀

## Welcome to Professional Vue Development!

Congratulations! You've mastered Vue fundamentals with CDN. Now it's time to level up to **professional Vue development** with real build tools, Single File Components, and modern development workflows. This is how Vue apps are built in the real world!

## What You'll Discover Today
- **Professional project setup** with modern build tools
- **Single File Components (SFCs)** - the `.vue` file magic
- **Hot reload development** - see changes instantly
- **Modern JavaScript tooling** - imports, modules, and more
- **Production builds** - optimized code for deployment

## The Big Transition: From CDN to Build Tools

### 🎯 What You've Been Doing (CDN)
```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<div id="app">{{ message }}</div>
<script>
  const { createApp, ref } = Vue
  // Vue code here...
</script>
```

### 🚀 What You're Moving To (Build Tools)
```vue
<!-- HelloWorld.vue -->
<template>
  <div>{{ message }}</div>
</template>

<script setup>
import { ref } from 'vue'
const message = ref('Hello from SFC!')
</script>

<style scoped>
div { color: blue; }
</style>
```

**Everything in one file - template, script, and styles!**

## Prerequisites - Let's Get Ready!

### 1. Install Node.js 📦
You need Node.js (version 18.3 or higher) to use modern Vue development tools.

**Download Node.js:**
- Go to [nodejs.org](https://nodejs.org/)
- Download the LTS (Long Term Support) version
- Install it (accept all defaults)

**Check if it's installed:**
```bash
node --version
npm --version
```

You should see version numbers like `v20.x.x` and `10.x.x`.

### 2. Choose Your Terminal 💻
- **Windows**: Command Prompt, PowerShell, or Git Bash
- **Mac**: Terminal app
- **Linux**: Your preferred terminal

### 3. Pick a Code Editor 🎨
**Recommended**: Visual Studio Code + Vue - Official extension
- Download VS Code from [code.visualstudio.com](https://code.visualstudio.com/)
- Install the "Vue - Official" extension for amazing Vue support

## Creating Your First Vue Project

### Step 1: Create the Project 🎯

Open your terminal and navigate to where you want to create your project:

```bash
# Navigate to your desired folder
cd Desktop
# or wherever you want to create your project

# Create a new Vue project (this is the official command!)
npm create vue@latest
```

### Step 2: Project Configuration Wizard 🧙‍♂️

You'll see a friendly setup wizard! Here's what each option means:

```
✔ Project name: … my-vue-app
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes  
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit testing? … No / Yes
✔ Add an End-to-End Testing Solution? … No / Cypress / Nightwatch / Playwright
✔ Add ESLint for code quality? … No / Yes
✔ Add Prettier for code formatting? … No / Yes
✔ Add Vue DevTools 7 extension for debugging? (experimental) … No / Yes
```

**🔰 Beginner Recommendation:**
```
✔ Project name: … my-first-vue-app
✔ Add TypeScript? … No (for now)
✔ Add JSX Support? … No
✔ Add Vue Router for Single Page Application development? … Yes (useful!)
✔ Add Pinia for state management? … No (learn this later)
✔ Add Vitest for Unit testing? … No (learn this later)
✔ Add an End-to-End Testing Solution? … No
✔ Add ESLint for code quality? … Yes (catches errors!)
✔ Add Prettier for code formatting? … Yes (makes code pretty!)
✔ Add Vue DevTools 7 extension for debugging? … Yes
```

**Why these choices?**
- **Vue Router**: Helps you create multi-page apps (like Home, About, Contact pages)
- **ESLint**: Catches coding mistakes before they become bugs
- **Prettier**: Automatically formats your code to look professional
- **Vue DevTools**: Amazing browser extension for debugging Vue apps

### Step 3: Install and Run 🏃‍♂️

```bash
# Navigate into your new project
cd my-first-vue-app

# Install all the dependencies (this might take a minute)
npm install

# Start the development server
npm run dev
```

🎉 **Success!** You should see something like:
```
  VITE v5.x.x  ready in 500 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help
```

Open `http://localhost:5173/` in your browser and see your Vue app!

## Understanding Your New Project Structure

```
my-first-vue-app/
├── public/
│   └── favicon.ico           # Website icon
├── src/
│   ├── assets/              # Images, fonts, etc.
│   ├── components/          # Reusable Vue components
│   │   └── HelloWorld.vue
│   ├── router/              # Page routing setup
│   │   └── index.js
│   ├── views/               # Full page components
│   │   ├── HomeView.vue
│   │   └── AboutView.vue
│   ├── App.vue              # Root component
│   ├── main.js              # App entry point
│   └── style.css            # Global styles
├── index.html               # Main HTML file
├── package.json             # Project dependencies and scripts
├── README.md                # Project documentation
└── vite.config.js           # Build tool configuration
```

### 📁 Key Folders Explained:

**`src/`** - All your Vue code lives here
- **`components/`** - Small, reusable pieces (like buttons, cards)
- **`views/`** - Full pages (like Home page, About page)
- **`App.vue`** - The main component that wraps everything
- **`main.js`** - Where Vue starts up

**`public/`** - Static files that don't get processed (images, favicon)

## Your First Single File Component (SFC)

Let's explore `src/components/HelloWorld.vue`:

```vue
<template>
  <!-- HTML template goes here -->
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      You've successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
    </h3>
  </div>
</template>

<script setup>
// JavaScript logic goes here
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<style scoped>
/* CSS styles go here */
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

.green {
  color: hsla(160, 100%, 37%, 1);
}
</style>
```

### 🎯 The Three Sections:

1. **`<template>`** - Your HTML (what users see)
2. **`<script setup>`** - Your JavaScript logic (what makes it work)
3. **`<style scoped>`** - Your CSS (how it looks)

**The magic**: All three parts work together in one file!

## Key Benefits of This Setup

### 🔥 Hot Module Replacement (HMR)
Change your code and see updates instantly in the browser - no page refresh needed!

### 📦 ES Modules & Imports
```javascript
// Import Vue functions
import { ref, computed } from 'vue'

// Import your own components
import HelloWorld from './components/HelloWorld.vue'

// Import CSS
import './style.css'
```

### 🎨 Scoped CSS
Styles in one component don't affect other components:
```vue
<style scoped>
.my-button {
  color: red; /* Only affects this component! */
}
</style>
```

### ⚡ Optimized Builds
When you run `npm run build`, you get:
- Minified code (smaller file sizes)
- Dead code elimination (removes unused code)
- Modern JavaScript that runs fast in browsers

## Essential Commands You'll Use

```bash
# Start development server (use this while coding)
npm run dev

# Build for production (when ready to deploy)
npm run build

# Preview production build locally
npm run preview

# Check code quality (find potential issues)
npm run lint

# Format code automatically
npm run format
```

## Creating Your First Component

Let's create a simple counter component to practice!

### 1. Create `src/components/MyCounter.vue`:

```vue
<template>
  <div class="counter">
    <h2>My Counter Component</h2>
    <div class="count-display">{{ count }}</div>
    <div class="buttons">
      <button @click="decrement" :disabled="count <= 0">-</button>
      <button @click="increment">+</button>
      <button @click="reset">Reset</button>
    </div>
    <p>You've clicked {{ totalClicks }} times total!</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Reactive state
const count = ref(0)
const clicks = ref(0)

// Computed property
const totalClicks = computed(() => clicks.value)

// Methods
const increment = () => {
  count.value++
  clicks.value++
}

const decrement = () => {
  if (count.value > 0) {
    count.value--
    clicks.value++
  }
}

const reset = () => {
  count.value = 0
  clicks.value++
}
</script>

<style scoped>
.counter {
  text-align: center;
  padding: 2rem;
  border: 2px solid #42b883;
  border-radius: 10px;
  margin: 1rem;
}

.count-display {
  font-size: 3rem;
  font-weight: bold;
  color: #42b883;
  margin: 1rem 0;
}

.buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin: 1rem 0;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1.1rem;
  border: 2px solid #42b883;
  background: white;
  color: #42b883;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.2s;
}

button:hover:not(:disabled) {
  background: #42b883;
  color: white;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
```

### 2. Use it in `src/views/HomeView.vue`:

```vue
<template>
  <main>
    <MyCounter />
  </main>
</template>

<script setup>
import MyCounter from '../components/MyCounter.vue'
</script>
```

Save both files and watch your browser update instantly! 🎉

## Development Workflow

### 1. **Code → Save → See Changes**
- Edit your `.vue` files
- Save (Ctrl/Cmd + S)
- Browser updates automatically!

### 2. **Component-Based Development**
- Build small, reusable components
- Compose them into larger features
- Import and use them anywhere

### 3. **Modern JavaScript**
- Use ES6+ features freely
- Import/export modules
- Build tools handle browser compatibility

## Common Issues and Solutions

### ❌ "npm is not recognized"
**Problem**: Node.js not installed properly  
**Solution**: Download and reinstall Node.js from nodejs.org

### ❌ Port 5173 already in use
**Problem**: Another app is using the same port  
**Solution**: Close other development servers or use `npm run dev -- --port 3000`

### ❌ Component not showing up
**Problem**: Import/export issues  
**Solution**: Check:
- File path in import statement
- Component is exported properly
- Component is registered in template

### ❌ Styles not working
**Problem**: CSS not loading or scoped incorrectly  
**Solution**: 
- Make sure `<style scoped>` is used for component-specific styles
- Check for typos in class names
- Use global styles in `src/style.css` for app-wide styles

## VS Code Extensions for Vue

Install these extensions for the best development experience:

1. **Vue - Official** (essential!)
2. **Vetur** (if using Vue 2)
3. **Auto Rename Tag**
4. **Bracket Pair Colorizer**
5. **ES6 String HTML** (for template syntax highlighting)

## What You've Accomplished! 🏆

- ✅ **Professional setup** with modern build tools
- ✅ **Single File Components** - organized, maintainable code
- ✅ **Hot reload development** - instant feedback while coding
- ✅ **Modern JavaScript** - imports, modules, ES6+ features
- ✅ **Production builds** - optimized code for deployment
- ✅ **Component architecture** - building apps the Vue way

## The Difference This Makes

### Before (CDN):
- Everything in one HTML file
- Global scope issues
- Manual script tags
- No build optimization
- Limited organization

### Now (Build Tools):
- Organized component files
- Proper imports/exports
- Automatic optimization
- Hot reload development
- Professional workflows

## What's Next?

In the next project, you'll learn about **Vite** - the lightning-fast build tool that powers your development experience. You'll understand why modern Vue development is so much faster and more enjoyable than traditional approaches!

You're now developing Vue the way professionals do it! 🚀

## Quick Reference

### Essential Commands:
```bash
npm create vue@latest     # Create new project
npm install              # Install dependencies  
npm run dev              # Start development
npm run build            # Build for production
```

### File Structure:
- **`.vue` files** - Single File Components
- **`src/components/`** - Reusable components
- **`src/views/`** - Page components
- **`src/App.vue`** - Root component
- **`src/main.js`** - Application entry point

### Import Patterns:
```javascript
// Vue functions
import { ref, computed } from 'vue'

// Components
import MyComponent from './components/MyComponent.vue'

// Styles
import './style.css'
```

You're ready to build real Vue applications! 🌟