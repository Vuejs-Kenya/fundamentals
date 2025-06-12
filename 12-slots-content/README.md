# 12 - Slots & Content Distribution: Flexible Components! 🎭

## Welcome to Component Flexibility!

You've mastered component communication and lifecycle management. Now it's time to learn about **slots** - Vue's powerful system for creating flexible, reusable components that can accept and display dynamic content. Think of slots as "content holes" in your components that parents can fill with whatever they want!

## What You'll Master Today
- **Default slots** - basic content projection
- **Named slots** - multiple content areas in one component
- **Scoped slots** - sharing data from child to parent content
- **Conditional slots** - smart content handling
- **Real-world patterns** - building truly flexible component libraries

## Understanding Slots: The Analogy

Think of slots like **picture frames**:
- The **frame** (your component) provides structure and styling
- The **picture** (slot content) is provided by whoever uses the frame
- You can have **multiple frames** (named slots) for different pictures
- The frame can even **give information** (scoped slots) about how to display the picture

## Complete Slots Demonstration

Let's build a comprehensive component library that showcases all slot features:

```vue
<!-- App.vue - Slots Showcase -->
<template>
  <div id="app">
    <h1>🎭 Vue Slots Mastery Showcase</h1>

    <!-- Section 1: Basic Slots -->
    <section class="demo-section">
      <h2>🎯 Basic Slots (Default Content Projection)</h2>
      
      <div class="examples-grid">
        <!-- Simple button with slot content -->
        <div class="example">
          <h3>Simple Button</h3>
          <FancyButton>
            Click Me! 🚀
          </FancyButton>
          
          <FancyButton>
            <span style="color: red;">❤️ Love this!</span>
          </FancyButton>
          
          <!-- Using fallback content -->
          <FancyButton />
        </div>

        <!-- Card with slot content -->
        <div class="example">
          <h3>Content Card</h3>
          <ContentCard>
            <h4>My Amazing Article</h4>
            <p>This is some amazing content that goes inside the card!</p>
            <p>It can be <strong>any HTML content</strong> at all.</p>
          </ContentCard>
        </div>
      </div>
    </section>

    <!-- Section 2: Named Slots -->
    <section class="demo-section">
      <h2>🏷️ Named Slots (Multiple Content Areas)</h2>
      
      <div class="examples-grid">
        <!-- Layout with multiple named slots -->
        <div class="example">
          <h3>Page Layout</h3>
          <PageLayout>
            <template #header>
              <h1>🌟 My Website Header</h1>
              <nav>
                <a href="#">Home</a> | 
                <a href="#">About</a> | 
                <a href="#">Contact</a>
              </nav>
            </template>

            <template #sidebar>
              <h3>📋 Sidebar Menu</h3>
              <ul>
                <li><a href="#">Menu Item 1</a></li>
                <li><a href="#">Menu Item 2</a></li>
                <li><a href="#">Menu Item 3</a></li>
              </ul>
            </template>

            <!-- Default slot content -->
            <h2>📄 Main Content Area</h2>
            <p>This is the main content of the page.</p>
            <p>It goes in the default slot automatically!</p>

            <template #footer>
              <p>© 2024 My Website. All rights reserved.</p>
            </template>
          </PageLayout>
        </div>

        <!-- Modal with named slots -->
        <div class="example">
          <h3>Modal Dialog</h3>
          <button @click="showModal = true" class="show-modal-btn">
            Show Modal
          </button>
          
          <ModalDialog v-if="showModal" @close="showModal = false">
            <template #title>
              <span>⚠️ Confirm Action</span>
            </template>

            <template #content>
              <p>Are you sure you want to delete this item?</p>
              <p>This action cannot be undone!</p>
            </template>

            <template #actions>
              <button @click="showModal = false" class="btn-cancel">
                Cancel
              </button>
              <button @click="confirmDelete" class="btn-danger">
                Delete
              </button>
            </template>
          </ModalDialog>
        </div>
      </div>
    </section>

    <!-- Section 3: Scoped Slots -->
    <section class="demo-section">
      <h2>🔗 Scoped Slots (Data Sharing)</h2>
      
      <div class="examples-grid">
        <!-- User list with scoped slots -->
        <div class="example">
          <h3>User List (Different Layouts)</h3>
          
          <!-- Card layout -->
          <h4>Card Layout:</h4>
          <UserList :users="users">
            <template #user="{ user, index, isOnline }">
              <div class="user-card">
                <img :src="user.avatar" :alt="user.name" class="avatar">
                <div class="user-info">
                  <h4>{{ user.name }}</h4>
                  <p>{{ user.email }}</p>
                  <span :class="['status', { online: isOnline }]">
                    {{ isOnline ? '🟢 Online' : '🔴 Offline' }}
                  </span>
                  <small>User #{{ index + 1 }}</small>
                </div>
              </div>
            </template>
          </UserList>

          <!-- Table layout -->
          <h4>Table Layout:</h4>
          <UserList :users="users">
            <template #user="{ user, index, isOnline }">
              <tr>
                <td>{{ index + 1 }}</td>
                <td>
                  <img :src="user.avatar" :alt="user.name" class="avatar-small">
                  {{ user.name }}
                </td>
                <td>{{ user.email }}</td>
                <td :class="{ online: isOnline, offline: !isOnline }">
                  {{ isOnline ? 'Online' : 'Offline' }}
                </td>
              </tr>
            </template>
          </UserList>
        </div>

        <!-- Data fetcher with scoped slots -->
        <div class="example">
          <h3>Data Fetcher</h3>
          <DataFetcher url="/api/posts">
            <template #loading>
              <div class="loading">🔄 Loading awesome content...</div>
            </template>

            <template #error="{ error }">
              <div class="error">
                ❌ Error: {{ error.message }}
                <button @click="retry">Try Again</button>
              </div>
            </template>

            <template #data="{ data, isLoading, refetch }">
              <div class="data-display">
                <h4>📊 Fetched Data ({{ data.length }} items)</h4>
                <button @click="refetch" :disabled="isLoading">
                  {{ isLoading ? 'Refreshing...' : '🔄 Refresh' }}
                </button>
                <ul>
                  <li v-for="item in data" :key="item.id">
                    <strong>{{ item.title }}</strong>
                    <p>{{ item.content.substring(0, 100) }}...</p>
                  </li>
                </ul>
              </div>
            </template>
          </DataFetcher>
        </div>
      </div>
    </section>

    <!-- Section 4: Advanced Slot Patterns -->
    <section class="demo-section">
      <h2>🚀 Advanced Slot Patterns</h2>
      
      <div class="examples-grid">
        <!-- Renderless component -->
        <div class="example">
          <h3>Mouse Tracker (Renderless)</h3>
          <MouseTracker>
            <template #default="{ x, y, isInside }">
              <div class="mouse-display">
                <p>🖱️ Mouse Position: ({{ x }}, {{ y }})</p>
                <p>📍 Mouse is {{ isInside ? 'inside' : 'outside' }} the area</p>
                <div 
                  class="mouse-indicator" 
                  :style="{ 
                    left: x + 'px', 
                    top: y + 'px',
                    opacity: isInside ? 1 : 0.3
                  }"
                ></div>
              </div>
            </template>
          </MouseTracker>
        </div>

        <!-- Form builder with slots -->
        <div class="example">
          <h3>Flexible Form Builder</h3>
          <FormBuilder @submit="handleFormSubmit">
            <template #title>
              <h3>🔐 Login Form</h3>
            </template>

            <template #fields>
              <div class="form-field">
                <label>Email:</label>
                <input v-model="formData.email" type="email" required>
              </div>
              <div class="form-field">
                <label>Password:</label>
                <input v-model="formData.password" type="password" required>
              </div>
            </template>

            <template #actions="{ isValid, reset }">
              <button type="submit" :disabled="!isValid" class="btn-primary">
                Login
              </button>
              <button type="button" @click="reset" class="btn-secondary">
                Reset
              </button>
            </template>

            <template #footer>
              <p><a href="#">Forgot your password?</a></p>
            </template>
          </FormBuilder>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import FancyButton from './components/FancyButton.vue'
import ContentCard from './components/ContentCard.vue'
import PageLayout from './components/PageLayout.vue'
import ModalDialog from './components/ModalDialog.vue'
import UserList from './components/UserList.vue'
import DataFetcher from './components/DataFetcher.vue'
import MouseTracker from './components/MouseTracker.vue'
import FormBuilder from './components/FormBuilder.vue'

// App state
const showModal = ref(false)
const users = ref([
  { 
    id: 1, 
    name: 'Alice Johnson', 
    email: 'alice@example.com', 
    avatar: '👩‍💼',
    lastSeen: Date.now() - 300000 // 5 minutes ago
  },
  { 
    id: 2, 
    name: 'Bob Smith', 
    email: 'bob@example.com', 
    avatar: '👨‍💻',
    lastSeen: Date.now() - 3600000 // 1 hour ago
  },
  { 
    id: 3, 
    name: 'Carol Brown', 
    email: 'carol@example.com', 
    avatar: '👩‍🎨',
    lastSeen: Date.now() - 60000 // 1 minute ago
  }
])

const formData = reactive({
  email: '',
  password: ''
})

// Methods
const confirmDelete = () => {
  alert('Item deleted! (This is just a demo)')
  showModal.value = false
}

const handleFormSubmit = (data) => {
  console.log('Form submitted:', data)
  alert('Login successful! (This is just a demo)')
}
</script>

<style>
#app {
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 40px;
}

.demo-section {
  margin-bottom: 50px;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 12px;
  border: 2px solid #e9ecef;
}

.demo-section h2 {
  margin-top: 0;
  color: #495057;
  border-bottom: 2px solid #dee2e6;
  padding-bottom: 10px;
}

.examples-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 30px;
  margin-top: 20px;
}

.example {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.example h3 {
  margin-top: 0;
  color: #007bff;
  border-bottom: 1px solid #e9ecef;
  padding-bottom: 8px;
}

.show-modal-btn {
  background: #007bff;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.user-card {
  display: flex;
  gap: 15px;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin-bottom: 10px;
  background: #f8f9fa;
}

.avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  font-size: 2em;
  display: flex;
  align-items: center;
  justify-content: center;
  background: white;
  border: 2px solid #dee2e6;
}

.avatar-small {
  width: 30px;
  height: 30px;
  font-size: 1em;
}

.user-info h4 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.user-info p {
  margin: 0 0 5px 0;
  color: #6c757d;
}

.status {
  font-size: 0.9em;
  padding: 2px 8px;
  border-radius: 12px;
  background: #f8d7da;
  color: #721c24;
}

.status.online {
  background: #d4edda;
  color: #155724;
}

.loading {
  text-align: center;
  padding: 40px;
  color: #007bff;
  font-size: 1.2em;
}

.error {
  text-align: center;
  padding: 20px;
  background: #f8d7da;
  color: #721c24;
  border-radius: 4px;
}

.data-display {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.mouse-display {
  position: relative;
  height: 200px;
  background: #f0f0f0;
  border: 2px dashed #ccc;
  border-radius: 8px;
  padding: 20px;
}

.mouse-indicator {
  position: absolute;
  width: 10px;
  height: 10px;
  background: #007bff;
  border-radius: 50%;
  pointer-events: none;
  transition: opacity 0.2s;
}

.form-field {
  margin-bottom: 15px;
}

.form-field label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #495057;
}

.form-field input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 4px;
  font-size: 16px;
}

.btn-primary {
  background: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.btn-secondary {
  background: #6c757d;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-cancel {
  background: #6c757d;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.btn-danger {
  background: #dc3545;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

th, td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background: #f8f9fa;
  font-weight: bold;
}

.online {
  color: #28a745;
}

.offline {
  color: #dc3545;
}

@media (max-width: 768px) {
  .examples-grid {
    grid-template-columns: 1fr;
  }
}
</style>
```

Now let's create all the component examples:

### 1. FancyButton.vue - Basic Slot
```vue
<template>
  <button class="fancy-btn">
    <slot>Default Button Text</slot>
  </button>
</template>

<style scoped>
.fancy-btn {
  background: linear-gradient(45deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 25px;
  cursor: pointer;
  font-weight: bold;
  font-size: 16px;
  transition: transform 0.2s, box-shadow 0.2s;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

.fancy-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.3);
}

.fancy-btn:active {
  transform: translateY(0);
}
</style>
```

### 2. ContentCard.vue - Basic Slot with Styling
```vue
<template>
  <div class="content-card">
    <slot>
      <p>No content provided. Add some content to see it here!</p>
    </slot>
  </div>
</template>

<style scoped>
.content-card {
  background: white;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  border: 1px solid #e9ecef;
  transition: transform 0.2s, box-shadow 0.2s;
}

.content-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 30px rgba(0,0,0,0.15);
}
</style>
```

### 3. PageLayout.vue - Named Slots
```vue
<template>
  <div class="page-layout">
    <header v-if="$slots.header" class="layout-header">
      <slot name="header"></slot>
    </header>
    
    <div class="layout-body">
      <aside v-if="$slots.sidebar" class="layout-sidebar">
        <slot name="sidebar"></slot>
      </aside>
      
      <main class="layout-main">
        <slot></slot>
      </main>
    </div>
    
    <footer v-if="$slots.footer" class="layout-footer">
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<style scoped>
.page-layout {
  border: 2px solid #dee2e6;
  border-radius: 8px;
  overflow: hidden;
  background: white;
}

.layout-header {
  background: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
  padding: 20px;
}

.layout-body {
  display: flex;
  min-height: 300px;
}

.layout-sidebar {
  width: 200px;
  background: #f8f9fa;
  border-right: 1px solid #dee2e6;
  padding: 20px;
}

.layout-main {
  flex: 1;
  padding: 20px;
}

.layout-footer {
  background: #f8f9fa;
  border-top: 1px solid #dee2e6;
  padding: 15px 20px;
  text-align: center;
  color: #6c757d;
}
</style>
```

### 4. ModalDialog.vue - Named Slots with Events
```vue
<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-dialog">
      <header v-if="$slots.title" class="modal-header">
        <slot name="title"></slot>
        <button @click="$emit('close')" class="close-btn">×</button>
      </header>
      
      <div v-if="$slots.content" class="modal-content">
        <slot name="content"></slot>
      </div>
      
      <div class="modal-body">
        <slot></slot>
      </div>
      
      <footer v-if="$slots.actions" class="modal-footer">
        <slot name="actions"></slot>
      </footer>
    </div>
  </div>
</template>

<script setup>
defineEmits(['close'])
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-dialog {
  background: white;
  border-radius: 8px;
  box-shadow: 0 10px 40px rgba(0,0,0,0.3);
  max-width: 500px;
  width: 90%;
  max-height: 90vh;
  overflow: hidden;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border-bottom: 1px solid #dee2e6;
  background: #f8f9fa;
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #6c757d;
  padding: 0;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  padding: 20px;
}

.modal-body {
  padding: 20px;
}

.modal-footer {
  padding: 15px 20px;
  background: #f8f9fa;
  border-top: 1px solid #dee2e6;
  text-align: right;
}
</style>
```

### 5. UserList.vue - Scoped Slots
```vue
<template>
  <div class="user-list">
    <div v-if="displayMode === 'table'">
      <table>
        <thead>
          <tr>
            <th>#</th>
            <th>User</th>
            <th>Email</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <slot 
            v-for="(user, index) in users" 
            name="user"
            :user="user"
            :index="index"
            :isOnline="isUserOnline(user)"
            :key="user.id"
          ></slot>
        </tbody>
      </table>
    </div>
    
    <div v-else class="user-cards">
      <slot 
        v-for="(user, index) in users" 
        name="user"
        :user="user"
        :index="index"
        :isOnline="isUserOnline(user)"
        :key="user.id"
      ></slot>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  users: {
    type: Array,
    required: true
  },
  displayMode: {
    type: String,
    default: 'cards' // 'cards' or 'table'
  }
})

// Check if user is online (last seen within 10 minutes)
const isUserOnline = (user) => {
  const tenMinutesAgo = Date.now() - 600000
  return user.lastSeen > tenMinutesAgo
}
</script>

<style scoped>
.user-list {
  margin: 15px 0;
}

.user-cards {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
```

### 6. DataFetcher.vue - Advanced Scoped Slots
```vue
<template>
  <div class="data-fetcher">
    <slot v-if="loading" name="loading"></slot>
    
    <slot 
      v-else-if="error" 
      name="error" 
      :error="error"
      :retry="fetchData"
    ></slot>
    
    <slot 
      v-else-if="data" 
      name="data"
      :data="data"
      :isLoading="loading"
      :refetch="fetchData"
      :lastFetched="lastFetched"
    ></slot>
    
    <div v-else>
      <p>No data available</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  url: {
    type: String,
    required: true
  },
  autoFetch: {
    type: Boolean,
    default: true
  }
})

const data = ref(null)
const loading = ref(false)
const error = ref(null)
const lastFetched = ref(null)

const fetchData = async () => {
  loading.value = true
  error.value = null
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // Mock data based on URL
    if (props.url.includes('posts')) {
      data.value = [
        { id: 1, title: 'First Post', content: 'This is the content of the first post. It contains some interesting information about Vue.js and how awesome it is for building user interfaces.' },
        { id: 2, title: 'Second Post', content: 'Here is another post with different content. This one talks about the benefits of using slots in Vue components.' },
        { id: 3, title: 'Third Post', content: 'The final post in our demo data set. This post covers advanced Vue patterns and best practices.' }
      ]
    } else {
      data.value = [
        { id: 1, name: 'Item 1', value: 100 },
        { id: 2, name: 'Item 2', value: 200 }
      ]
    }
    
    lastFetched.value = new Date()
  } catch (err) {
    error.value = err
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  if (props.autoFetch) {
    fetchData()
  }
})
</script>

<style scoped>
.data-fetcher {
  min-height: 100px;
}
</style>
```

### 7. MouseTracker.vue - Renderless Component
```vue
<template>
  <div 
    ref="trackerRef"
    class="mouse-tracker"
    @mousemove="updatePosition"
    @mouseenter="isInside = true"
    @mouseleave="isInside = false"
  >
    <slot 
      :x="relativeX" 
      :y="relativeY" 
      :isInside="isInside"
      :absoluteX="absoluteX"
      :absoluteY="absoluteY"
    ></slot>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const trackerRef = ref(null)
const relativeX = ref(0)
const relativeY = ref(0)
const absoluteX = ref(0)
const absoluteY = ref(0)
const isInside = ref(false)

const updatePosition = (event) => {
  absoluteX.value = event.clientX
  absoluteY.value = event.clientY
  
  if (trackerRef.value) {
    const rect = trackerRef.value.getBoundingClientRect()
    relativeX.value = event.clientX - rect.left
    relativeY.value = event.clientY - rect.top
  }
}
</script>

<style scoped>
.mouse-tracker {
  width: 100%;
  height: 100%;
  min-height: 200px;
}
</style>
```

### 8. FormBuilder.vue - Complex Named and Scoped Slots
```vue
<template>
  <form @submit.prevent="handleSubmit" class="form-builder">
    <header v-if="$slots.title" class="form-title">
      <slot name="title"></slot>
    </header>
    
    <div v-if="$slots.fields" class="form-fields">
      <slot name="fields"></slot>
    </div>
    
    <div class="form-body">
      <slot></slot>
    </div>
    
    <div v-if="$slots.actions" class="form-actions">
      <slot 
        name="actions"
        :isValid="isValid"
        :reset="resetForm"
        :submit="handleSubmit"
        :errors="errors"
      ></slot>
    </div>
    
    <footer v-if="$slots.footer" class="form-footer">
      <slot name="footer"></slot>
    </footer>
  </form>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['submit'])

const errors = ref([])
const isSubmitting = ref(false)

// Simple form validation (in real app, this would be more sophisticated)
const isValid = computed(() => {
  // Basic validation - check if form has required fields filled
  const form = document.querySelector('.form-builder')
  if (!form) return false
  
  const requiredInputs = form.querySelectorAll('input[required]')
  return Array.from(requiredInputs).every(input => input.value.trim() !== '')
})

const handleSubmit = () => {
  if (!isValid.value) {
    errors.value = ['Please fill in all required fields']
    return
  }
  
  isSubmitting.value = true
  errors.value = []
  
  // Collect form data
  const form = document.querySelector('.form-builder')
  const formData = new FormData(form)
  const data = Object.fromEntries(formData.entries())
  
  emit('submit', data)
  
  setTimeout(() => {
    isSubmitting.value = false
  }, 1000)
}

const resetForm = () => {
  const form = document.querySelector('.form-builder')
  if (form) {
    form.reset()
  }
  errors.value = []
}
</script>

<style scoped>
.form-builder {
  background: white;
  border-radius: 8px;
  border: 1px solid #dee2e6;
  overflow: hidden;
}

.form-title {
  background: #f8f9fa;
  padding: 20px;
  border-bottom: 1px solid #dee2e6;
}

.form-fields {
  padding: 20px;
}

.form-body {
  padding: 20px;
}

.form-actions {
  padding: 15px 20px;
  background: #f8f9fa;
  border-top: 1px solid #dee2e6;
}

.form-footer {
  padding: 15px 20px;
  text-align: center;
  color: #6c757d;
  border-top: 1px solid #dee2e6;
}
</style>
```

## Understanding Different Types of Slots

### 🎯 **Default Slots: The Basics**

Default slots are the simplest form - content goes in, component wraps it:

```vue
<!-- Component definition -->
<template>
  <button class="fancy-btn">
    <slot>Default Text</slot>  <!-- Fallback content -->
  </button>
</template>

<!-- Usage -->
<FancyButton>Click Me!</FancyButton>
<!-- Result: <button class="fancy-btn">Click Me!</button> -->

<FancyButton />
<!-- Result: <button class="fancy-btn">Default Text</button> -->
```

**Key concepts:**
- **Content projection** - parent content appears inside child component
- **Fallback content** - shown when no content is provided
- **Render scope** - slot content has access to parent data, not child data

### 🏷️ **Named Slots: Multiple Content Areas**

Named slots let you have multiple "holes" in your component:

```vue
<!-- Component definition -->
<template>
  <div class="layout">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>  <!-- Default slot -->
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<!-- Usage -->
<Layout>
  <template #header>
    <h1>Page Title</h1>
  </template>
  
  <p>Main content goes here</p>  <!-- Default slot -->
  
  <template #footer>
    <p>Footer content</p>
  </template>
</Layout>
```

**Key concepts:**
- **Multiple content areas** - organize complex layouts
- **Template syntax** - use `<template #name>` for named slots
- **Shorthand** - `#header` is short for `v-slot:header`
- **Default slot** - unnamed slot gets content not in templates

### 🔗 **Scoped Slots: Data Sharing**

Scoped slots let child components pass data to parent content:

```vue
<!-- Component definition -->
<template>
  <div>
    <slot 
      v-for="user in users" 
      :user="user" 
      :index="index"
      :isOnline="checkOnlineStatus(user)"
    ></slot>
  </div>
</template>

<!-- Usage -->
<UserList>
  <template #default="{ user, index, isOnline }">
    <div class="user-card">
      <h3>{{ user.name }}</h3>
      <span :class="{ online: isOnline }">
        {{ isOnline ? 'Online' : 'Offline' }}
      </span>
    </div>
  </template>
</UserList>
```

**Key concepts:**
- **Data flow reversal** - child passes data to parent content
- **Flexible rendering** - parent decides how to display child data
- **Reusability** - same data, different presentations

## Real-World Slot Patterns

### 1. **Layout Components**
```vue
<!-- AdminLayout.vue -->
<template>
  <div class="admin-layout">
    <aside class="sidebar">
      <slot name="sidebar">
        <DefaultNavigation />
      </slot>
    </aside>
    
    <main class="content">
      <header v-if="$slots.header">
        <slot name="header"></slot>
      </header>
      
      <div class="page-content">
        <slot></slot>
      </div>
    </main>
  </div>
</template>

<!-- Usage -->
<AdminLayout>
  <template #sidebar>
    <CustomNavigation />
  </template>
  
  <template #header>
    <PageHeader title="Dashboard" />
  </template>
  
  <DashboardContent />
</AdminLayout>
```

### 2. **Data Display Components**
```vue
<!-- DataTable.vue -->
<template>
  <table>
    <thead>
      <slot name="header" :columns="columns"></slot>
    </thead>
    <tbody>
      <slot 
        name="row"
        v-for="(item, index) in data"
        :item="item"
        :index="index"
        :key="item.id"
      ></slot>
    </tbody>
  </table>
</template>

<!-- Usage -->
<DataTable :data="users">
  <template #header="{ columns }">
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Actions</th>
    </tr>
  </template>
  
  <template #row="{ item, index }">
    <tr>
      <td>{{ item.name }}</td>
      <td>{{ item.email }}</td>
      <td>
        <button @click="edit(item)">Edit</button>
        <button @click="delete(item)">Delete</button>
      </td>
    </tr>
  </template>
</DataTable>
```

### 3. **State Management Components**
```vue
<!-- AsyncData.vue -->
<template>
  <div>
    <slot v-if="loading" name="loading">
      <div>Loading...</div>
    </slot>
    
    <slot 
      v-else-if="error" 
      name="error" 
      :error="error"
      :retry="fetchData"
    >
      <div>Error: {{ error.message }}</div>
    </slot>
    
    <slot 
      v-else-if="data"
      name="success"
      :data="data"
      :refetch="fetchData"
    ></slot>
  </div>
</template>

<!-- Usage -->
<AsyncData url="/api/users">
  <template #loading>
    <SkeletonLoader />
  </template>
  
  <template #error="{ error, retry }">
    <ErrorMessage :error="error" @retry="retry" />
  </template>
  
  <template #success="{ data, refetch }">
    <UserList :users="data" @refresh="refetch" />
  </template>
</AsyncData>
```

### 4. **Form Components**
```vue
<!-- FormField.vue -->
<template>
  <div class="form-field">
    <label v-if="$slots.label || label">
      <slot name="label">{{ label }}</slot>
    </label>
    
    <div class="input-wrapper">
      <slot 
        name="input"
        :value="modelValue"
        :updateValue="updateValue"
        :isValid="isValid"
        :error="error"
      >
        <input 
          :value="modelValue"
          @input="updateValue"
          :class="{ error: !isValid }"
        />
      </slot>
    </div>
    
    <div v-if="$slots.help || help" class="help-text">
      <slot name="help">{{ help }}</slot>
    </div>
    
    <div v-if="error" class="error-text">
      <slot name="error" :error="error">
        {{ error }}
      </slot>
    </div>
  </div>
</template>

<!-- Usage -->
<FormField v-model="email" label="Email" :rules="emailRules">
  <template #input="{ value, updateValue, isValid }">
    <div class="email-input">
      <EmailIcon />
      <input 
        :value="value"
        @input="updateValue"
        type="email"
        :class="{ valid: isValid }"
      />
    </div>
  </template>
  
  <template #help>
    We'll never share your email with anyone else.
  </template>
</FormField>
```

## Advanced Slot Techniques

### 1. **Conditional Slots with `$slots`**
```vue
<template>
  <div class="card">
    <!-- Only render header if content provided -->
    <header v-if="$slots.header" class="card-header">
      <slot name="header"></slot>
    </header>
    
    <div class="card-body">
      <slot></slot>
    </div>
    
    <!-- Only render footer if content provided -->
    <footer v-if="$slots.footer" class="card-footer">
      <slot name="footer"></slot>
    </footer>
  </div>
</template>
```

### 2. **Dynamic Slot Names**
```vue
<template>
  <div>
    <slot :name="dynamicSlotName">
      Default content for {{ dynamicSlotName }}
    </slot>
  </div>
</template>

<script setup>
const props = defineProps(['slotName'])
const dynamicSlotName = computed(() => props.slotName || 'default')
</script>

<!-- Usage -->
<DynamicSlot slot-name="header">
  <template #header>
    This goes in the header slot!
  </template>
</DynamicSlot>
```

### 3. **Slot Composition**
```vue
<!-- BaseModal.vue -->
<template>
  <div class="modal">
    <slot name="trigger" :open="openModal">
      <button @click="openModal">Open Modal</button>
    </slot>
    
    <div v-if="isOpen" class="modal-content">
      <slot :close="closeModal" :isOpen="isOpen"></slot>
    </div>
  </div>
</template>

<!-- Usage -->
<BaseModal>
  <template #trigger="{ open }">
    <button @click="open" class="custom-trigger">
      Custom Trigger
    </button>
  </template>
  
  <template #default="{ close, isOpen }">
    <h2>Modal Content</h2>
    <p>This is modal content</p>
    <button @click="close">Close</button>
  </template>
</BaseModal>
```

### 4. **Renderless Components**
```vue
<!-- WindowSize.vue - Renderless component -->
<template>
  <slot 
    :width="windowWidth" 
    :height="windowHeight"
    :isMobile="isMobile"
    :isTablet="isTablet"
    :isDesktop="isDesktop"
  ></slot>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const windowWidth = ref(window.innerWidth)
const windowHeight = ref(window.innerHeight)

const isMobile = computed(() => windowWidth.value < 768)
const isTablet = computed(() => windowWidth.value >= 768 && windowWidth.value < 1024)
const isDesktop = computed(() => windowWidth.value >= 1024)

const updateSize = () => {
  windowWidth.value = window.innerWidth
  windowHeight.value = window.innerHeight
}

onMounted(() => {
  window.addEventListener('resize', updateSize)
})

onUnmounted(() => {
  window.removeEventListener('resize', updateSize)
})
</script>

<!-- Usage -->
<WindowSize>
  <template #default="{ width, height, isMobile, isTablet, isDesktop }">
    <div>
      <p>Window size: {{ width }} × {{ height }}</p>
      <p v-if="isMobile">Mobile view</p>
      <p v-else-if="isTablet">Tablet view</p>
      <p v-else-if="isDesktop">Desktop view</p>
    </div>
  </template>
</WindowSize>
```

## Performance Considerations

### 1. **Slot Rendering Performance**
```vue
<!-- ✅ Good: Conditional rendering with v-if -->
<template>
  <div>
    <div v-if="$slots.header">
      <slot name="header"></slot>
    </div>
  </div>
</template>

<!-- ❌ Avoid: Always rendering empty slots -->
<template>
  <div>
    <div>
      <slot name="header"></slot>  <!-- Always renders wrapper -->
    </div>
  </div>
</template>
```

### 2. **Scoped Slot Performance**
```vue
<!-- ✅ Good: Computed properties for expensive calculations -->
<template>
  <slot 
    v-for="item in items"
    :item="item"
    :processedData="getProcessedData(item)"
    :key="item.id"
  ></slot>
</template>

<script setup>
const getProcessedData = computed(() => {
  return (item) => {
    // Expensive calculation cached per item
    return expensiveCalculation(item)
  }
})
</script>
```

## Testing Slots

### 1. **Testing Slot Content**
```javascript
import { mount } from '@vue/test-utils'
import FancyButton from '@/components/FancyButton.vue'

test('renders slot content', () => {
  const wrapper = mount(FancyButton, {
    slots: {
      default: 'Click me!'
    }
  })
  
  expect(wrapper.text()).toContain('Click me!')
})

test('renders fallback content when no slot provided', () => {
  const wrapper = mount(FancyButton)
  expect(wrapper.text()).toContain('Default Button Text')
})
```

### 2. **Testing Named Slots**
```javascript
test('renders named slots correctly', () => {
  const wrapper = mount(PageLayout, {
    slots: {
      header: '<h1>Page Title</h1>',
      default: '<p>Main content</p>',
      footer: '<p>Footer</p>'
    }
  })
  
  expect(wrapper.find('.layout-header').html()).toContain('<h1>Page Title</h1>')
  expect(wrapper.find('.layout-main').html()).toContain('<p>Main content</p>')
  expect(wrapper.find('.layout-footer').html()).toContain('<p>Footer</p>')
})
```

### 3. **Testing Scoped Slots**
```javascript
test('passes correct data to scoped slots', () => {
  const wrapper = mount(UserList, {
    props: {
      users: [{ id: 1, name: 'John', lastSeen: Date.now() }]
    },
    slots: {
      user: `
        <template #user="{ user, isOnline }">
          <div class="user">{{ user.name }} - {{ isOnline ? 'Online' : 'Offline' }}</div>
        </template>
      `
    }
  })
  
  expect(wrapper.html()).toContain('John - Online')
})
```

## What You've Mastered! 🏆

- ✅ **Default slots** - basic content projection with fallback content
- ✅ **Named slots** - multiple content areas in complex components
- ✅ **Scoped slots** - child-to-parent data sharing for flexible rendering
- ✅ **Conditional slots** - smart slot handling with `$slots`
- ✅ **Advanced patterns** - renderless components and dynamic slots
- ✅ **Real-world applications** - layout, data, form, and state components
- ✅ **Performance optimization** - efficient slot rendering techniques
- ✅ **Testing strategies** - validating slot behavior and content

## Best Practices Summary

### 1. **When to Use Each Slot Type**
- **Default slots** → Simple content projection (buttons, cards, wrappers)
- **Named slots** → Multiple content areas (layouts, modals, forms)  
- **Scoped slots** → Flexible data presentation (lists, tables, async components)

### 2. **Design Principles**
- **Provide fallback content** for better UX
- **Use `$slots` for conditional rendering** to avoid empty wrappers
- **Keep scoped slot data minimal** - only pass what's needed
- **Design for reusability** - think about different use cases

### 3. **Performance Tips**
- Use `v-if="$slots.slotName"` to conditionally render slot containers
- Avoid expensive calculations in scoped slot props
- Consider using computed properties for processed slot data
- Test slot performance with large datasets

## What's Next?

You've now mastered the art of flexible component design with slots! In the next project, you'll build a **complete todo application** that brings together all the Vue concepts you've learned - components, props, events, lifecycle hooks, computed properties, watchers, and slots!

## Quick Reference

### Slot Types:
```vue
<!-- Default Slot -->
<MyComponent>Content goes here</MyComponent>

<!-- Named Slots -->
<MyComponent>
  <template #header>Header content</template>
  <template #footer>Footer content</template>
</MyComponent>

<!-- Scoped Slots -->
<MyComponent>
  <template #item="{ item, index }">
    <div>{{ item.name }} (#{{ index }})</div>
  </template>
</MyComponent>

<!-- Conditional Slots -->
<div v-if="$slots.header">
  <slot name="header"></slot>
</div>
```

### Common Patterns:
- **Layout components** → Multiple named slots for different areas
- **Data components** → Scoped slots for flexible item rendering  
- **Form components** → Named slots for labels, inputs, help text
- **Modal components** → Named slots for title, content, actions
- **Renderless components** → Pure logic with scoped slot data

You now have the power to build incredibly flexible and reusable Vue components! 🌟