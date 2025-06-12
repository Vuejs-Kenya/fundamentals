# 11 - Lifecycle Hooks: Component Birth to Death! 🔄

## Welcome to the Component Lifecycle!

Every Vue component has a lifecycle - it's born, grows up, changes, and eventually gets destroyed. Lifecycle hooks are special functions that run at specific moments in this journey, letting you add your own code at exactly the right time. Think of them as event handlers for your component's life events!

## What You'll Master Today
- **Component lifecycle phases** - the journey from creation to destruction
- **Essential lifecycle hooks** - `onMounted`, `onUpdated`, `onUnmounted`, and more
- **When to use each hook** - timing your code perfectly
- **Real-world scenarios** - practical applications of lifecycle hooks
- **Cleanup patterns** - preventing memory leaks and resource issues

## The Component Lifecycle Journey

### 🌱 **Creation Phase**
Component is being born and set up
- **beforeCreate** → Basic instance initialization
- **created** → Data observation and event setup
- **onBeforeMount** → Just before mounting to DOM
- **onMounted** → Component is in the DOM and ready

### 🔄 **Update Phase** 
Component reacts to data changes
- **onBeforeUpdate** → Before re-rendering
- **onUpdated** → After re-rendering

### 💀 **Destruction Phase**
Component is being removed
- **onBeforeUnmount** → Cleanup time!
- **onUnmounted** → Component is gone

## Complete Lifecycle Demo Application

Let's build a comprehensive app that demonstrates all lifecycle hooks in action:

```vue
<!-- App.vue - Lifecycle Demo Controller -->
<template>
  <div id="app">
    <h1>🔄 Vue Lifecycle Hooks Demo</h1>
    
    <!-- Component Controls -->
    <div class="controls">
      <button @click="showComponent = !showComponent" class="toggle-btn">
        {{ showComponent ? '💀 Destroy Component' : '🌱 Create Component' }}
      </button>
      
      <button @click="updateCounter++" class="update-btn" :disabled="!showComponent">
        🔄 Trigger Update ({{ updateCounter }})
      </button>
      
      <button @click="resetDemo" class="reset-btn">
        🔄 Reset Demo
      </button>
    </div>

    <!-- Lifecycle Event Log -->
    <div class="lifecycle-log">
      <h2>📜 Lifecycle Event Log</h2>
      <button @click="clearLog" class="clear-log-btn">Clear Log</button>
      <div class="log-container">
        <div
          v-for="(event, index) in lifecycleEvents"
          :key="index"
          :class="['log-entry', `log-${event.type}`]"
        >
          <span class="log-time">{{ event.timestamp }}</span>
          <span class="log-hook">{{ event.hook }}</span>
          <span class="log-message">{{ event.message }}</span>
        </div>
      </div>
    </div>

    <!-- The Component Being Demonstrated -->
    <div class="component-container">
      <h2>🎭 Demo Component</h2>
      <LifecycleDemo
        v-if="showComponent"
        :update-trigger="updateCounter"
        :demo-data="demoData"
        @lifecycle-event="logLifecycleEvent"
        @data-loaded="handleDataLoaded"
        @cleanup-complete="handleCleanupComplete"
      />
      <div v-else class="component-placeholder">
        <p>💀 Component is destroyed</p>
        <p>Click "Create Component" to see it come to life!</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import LifecycleDemo from './components/LifecycleDemo.vue'

// App state
const showComponent = ref(true)
const updateCounter = ref(0)
const lifecycleEvents = ref([])

const demoData = reactive({
  user: { name: 'Vue Developer', id: 1 },
  preferences: { theme: 'light', notifications: true },
  lastUpdate: Date.now()
})

// Methods
const logLifecycleEvent = (event) => {
  lifecycleEvents.value.push({
    ...event,
    timestamp: new Date().toLocaleTimeString()
  })
  
  // Keep only last 50 events
  if (lifecycleEvents.value.length > 50) {
    lifecycleEvents.value = lifecycleEvents.value.slice(-50)
  }
}

const clearLog = () => {
  lifecycleEvents.value = []
}

const resetDemo = () => {
  showComponent.value = false
  updateCounter.value = 0
  lifecycleEvents.value = []
  demoData.lastUpdate = Date.now()
  
  // Recreate component after a brief delay
  setTimeout(() => {
    showComponent.value = true
  }, 100)
}

const handleDataLoaded = (data) => {
  console.log('📊 Parent received data loaded event:', data)
}

const handleCleanupComplete = () => {
  console.log('🧹 Parent received cleanup complete event')
}
</script>

<style>
#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
}

.controls {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.controls button {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  font-size: 16px;
  transition: all 0.2s;
}

.toggle-btn {
  background: #e74c3c;
  color: white;
}

.toggle-btn:hover {
  background: #c0392b;
}

.update-btn {
  background: #3498db;
  color: white;
}

.update-btn:hover:not(:disabled) {
  background: #2980b9;
}

.update-btn:disabled {
  background: #bdc3c7;
  cursor: not-allowed;
}

.reset-btn {
  background: #95a5a6;
  color: white;
}

.reset-btn:hover {
  background: #7f8c8d;
}

.lifecycle-log {
  background: #f8f9fa;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 30px;
  border: 2px solid #e9ecef;
}

.lifecycle-log h2 {
  margin-top: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.clear-log-btn {
  padding: 6px 12px;
  background: #6c757d;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.log-container {
  max-height: 300px;
  overflow-y: auto;
  background: white;
  border-radius: 4px;
  padding: 10px;
}

.log-entry {
  display: flex;
  gap: 15px;
  padding: 8px;
  border-bottom: 1px solid #e9ecef;
  font-family: 'Courier New', monospace;
  font-size: 14px;
}

.log-time {
  color: #6c757d;
  min-width: 80px;
}

.log-hook {
  font-weight: bold;
  min-width: 120px;
}

.log-creation { color: #28a745; }
.log-update { color: #007bff; }
.log-destruction { color: #dc3545; }
.log-effect { color: #6f42c1; }

.component-container {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.component-placeholder {
  text-align: center;
  padding: 40px;
  color: #6c757d;
  background: #f8f9fa;
  border-radius: 8px;
  border: 2px dashed #dee2e6;
}

@media (max-width: 768px) {
  .controls {
    flex-direction: column;
    align-items: center;
  }
  
  .controls button {
    width: 200px;
  }
}
</style>
```

Now let's create the demo component that showcases all lifecycle hooks:

```vue
<!-- LifecycleDemo.vue - The Component Being Demonstrated -->
<template>
  <div class="lifecycle-demo">
    <h3>🎯 I am the Demo Component!</h3>
    
    <!-- Component Status -->
    <div class="status-section">
      <div class="status-card">
        <h4>📊 Component Status</h4>
        <div class="status-grid">
          <div class="status-item">
            <span class="status-label">Mounted:</span>
            <span class="status-value">{{ isMounted ? '✅' : '❌' }}</span>
          </div>
          <div class="status-item">
            <span class="status-label">Data Loaded:</span>
            <span class="status-value">{{ isDataLoaded ? '✅' : '❌' }}</span>
          </div>
          <div class="status-item">
            <span class="status-label">Updates:</span>
            <span class="status-value">{{ updateCount }}</span>
          </div>
          <div class="status-item">
            <span class="status-label">API Calls:</span>
            <span class="status-value">{{ apiCallCount }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Interactive Elements -->
    <div class="interactive-section">
      <h4>🎮 Interactive Elements</h4>
      <div class="controls-grid">
        <button @click="localCounter++" class="local-btn">
          Local Counter: {{ localCounter }}
        </button>
        <button @click="fetchData" class="fetch-btn" :disabled="isLoading">
          {{ isLoading ? 'Loading...' : 'Fetch Data' }}
        </button>
        <button @click="startTimer" class="timer-btn" :disabled="timerActive">
          {{ timerActive ? `Timer: ${timerCount}s` : 'Start Timer' }}
        </button>
        <button @click="addEventListeners" class="events-btn">
          Add Window Events
        </button>
      </div>
    </div>

    <!-- Data Display -->
    <div class="data-section">
      <h4>📦 Component Data</h4>
      <div class="data-display">
        <div class="data-item">
          <strong>Update Trigger:</strong> {{ updateTrigger }}
        </div>
        <div class="data-item">
          <strong>Demo Data:</strong> {{ JSON.stringify(demoData, null, 2) }}
        </div>
        <div class="data-item">
          <strong>Fetched Data:</strong>
          <pre v-if="fetchedData">{{ JSON.stringify(fetchedData, null, 2) }}</pre>
          <span v-else>No data loaded</span>
        </div>
      </div>
    </div>

    <!-- Resource Monitor -->
    <div class="resources-section">
      <h4>🔧 Resources & Cleanup</h4>
      <div class="resources-list">
        <div class="resource-item">
          <span>🔄 Timer:</span>
          <span :class="{ active: timerActive }">{{ timerActive ? 'Active' : 'Inactive' }}</span>
        </div>
        <div class="resource-item">
          <span>👂 Event Listeners:</span>
          <span :class="{ active: hasEventListeners }">{{ eventListenerCount }} active</span>
        </div>
        <div class="resource-item">
          <span>🌐 API Subscriptions:</span>
          <span :class="{ active: hasApiSubscription }">{{ hasApiSubscription ? 'Active' : 'None' }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { 
  ref, 
  reactive, 
  computed, 
  watch, 
  onBeforeMount, 
  onMounted, 
  onBeforeUpdate, 
  onUpdated, 
  onBeforeUnmount, 
  onUnmounted,
  onActivated,
  onDeactivated,
  onErrorCaptured
} from 'vue'

// Props
const props = defineProps({
  updateTrigger: {
    type: Number,
    default: 0
  },
  demoData: {
    type: Object,
    default: () => ({})
  }
})

// Events
const emit = defineEmits([
  'lifecycle-event',
  'data-loaded',
  'cleanup-complete'
])

// Local state
const isMounted = ref(false)
const isDataLoaded = ref(false)
const updateCount = ref(0)
const apiCallCount = ref(0)
const localCounter = ref(0)
const isLoading = ref(false)
const fetchedData = ref(null)
const timerActive = ref(false)
const timerCount = ref(0)
const eventListenerCount = ref(0)
const hasEventListeners = ref(false)
const hasApiSubscription = ref(false)

// Resource references for cleanup
let timerId = null
let apiSubscription = null
const eventListeners = []

// Helper function to emit lifecycle events
const emitLifecycleEvent = (hook, message, type = 'creation') => {
  const event = {
    hook,
    message,
    type,
    componentId: 'LifecycleDemo'
  }
  
  emit('lifecycle-event', event)
  console.log(`🔄 ${hook}: ${message}`)
}

// 🌱 CREATION LIFECYCLE HOOKS

onBeforeMount(() => {
  emitLifecycleEvent(
    'onBeforeMount', 
    'Component is about to be mounted to the DOM', 
    'creation'
  )
})

onMounted(() => {
  isMounted.value = true
  emitLifecycleEvent(
    'onMounted', 
    'Component has been mounted to the DOM and is ready for interaction', 
    'creation'
  )
  
  // Simulate initial data loading
  setTimeout(() => {
    loadInitialData()
  }, 500)
  
  // Set up any DOM-dependent features
  setupDOMFeatures()
})

// 🔄 UPDATE LIFECYCLE HOOKS

onBeforeUpdate(() => {
  emitLifecycleEvent(
    'onBeforeUpdate', 
    'Component is about to re-render due to reactive data changes', 
    'update'
  )
})

onUpdated(() => {
  updateCount.value++
  emitLifecycleEvent(
    'onUpdated', 
    `Component has re-rendered (Update #${updateCount.value})`, 
    'update'
  )
})

// 💀 DESTRUCTION LIFECYCLE HOOKS

onBeforeUnmount(() => {
  emitLifecycleEvent(
    'onBeforeUnmount', 
    'Component is about to be destroyed - cleanup time!', 
    'destruction'
  )
  
  // Start cleanup process
  performCleanup()
})

onUnmounted(() => {
  emitLifecycleEvent(
    'onUnmounted', 
    'Component has been completely destroyed and removed from DOM', 
    'destruction'
  )
  
  emit('cleanup-complete')
})

// 🎭 SPECIAL LIFECYCLE HOOKS (for keep-alive components)

onActivated(() => {
  emitLifecycleEvent(
    'onActivated', 
    'Component was activated (used with <keep-alive>)', 
    'effect'
  )
})

onDeactivated(() => {
  emitLifecycleEvent(
    'onDeactivated', 
    'Component was deactivated (used with <keep-alive>)', 
    'effect'
  )
})

// 🚨 ERROR HANDLING HOOK

onErrorCaptured((error, instance, errorInfo) => {
  emitLifecycleEvent(
    'onErrorCaptured', 
    `Caught error: ${error.message}`, 
    'effect'
  )
  
  console.error('Component error captured:', { error, instance, errorInfo })
  return false // Prevent error from propagating
})

// 🎯 COMPONENT METHODS

const loadInitialData = async () => {
  isLoading.value = true
  apiCallCount.value++
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    fetchedData.value = {
      timestamp: new Date().toISOString(),
      userId: props.demoData.user?.id || 'unknown',
      preferences: { ...props.demoData.preferences },
      randomData: Math.random()
    }
    
    isDataLoaded.value = true
    emit('data-loaded', fetchedData.value)
    
    emitLifecycleEvent(
      'Data Loaded', 
      'Initial data has been successfully loaded', 
      'effect'
    )
  } catch (error) {
    console.error('Failed to load data:', error)
  } finally {
    isLoading.value = false
  }
}

const fetchData = async () => {
  isLoading.value = true
  apiCallCount.value++
  
  try {
    await new Promise(resolve => setTimeout(resolve, 800))
    
    fetchedData.value = {
      ...fetchedData.value,
      lastFetch: new Date().toISOString(),
      fetchCount: apiCallCount.value,
      randomValue: Math.floor(Math.random() * 1000)
    }
    
    emitLifecycleEvent(
      'Data Fetched', 
      `Data refreshed (API call #${apiCallCount.value})`, 
      'effect'
    )
  } finally {
    isLoading.value = false
  }
}

const startTimer = () => {
  if (timerActive.value) return
  
  timerActive.value = true
  timerCount.value = 0
  
  timerId = setInterval(() => {
    timerCount.value++
    
    if (timerCount.value >= 10) {
      clearInterval(timerId)
      timerActive.value = false
      timerId = null
    }
  }, 1000)
  
  emitLifecycleEvent(
    'Timer Started', 
    'Background timer has been started', 
    'effect'
  )
}

const addEventListeners = () => {
  const handleResize = () => {
    console.log('Window resized!')
  }
  
  const handleScroll = () => {
    console.log('Window scrolled!')
  }
  
  window.addEventListener('resize', handleResize)
  window.addEventListener('scroll', handleScroll)
  
  eventListeners.push(
    { event: 'resize', handler: handleResize },
    { event: 'scroll', handler: handleScroll }
  )
  
  eventListenerCount.value = eventListeners.length
  hasEventListeners.value = true
  
  emitLifecycleEvent(
    'Events Added', 
    `Added ${eventListeners.length} window event listeners`, 
    'effect'
  )
}

const setupDOMFeatures = () => {
  // Example of DOM-dependent setup that needs to happen after mounting
  emitLifecycleEvent(
    'DOM Setup', 
    'DOM-dependent features have been initialized', 
    'effect'
  )
}

const performCleanup = () => {
  // Clear timer
  if (timerId) {
    clearInterval(timerId)
    timerId = null
    timerActive.value = false
  }
  
  // Remove event listeners
  eventListeners.forEach(({ event, handler }) => {
    window.removeEventListener(event, handler)
  })
  eventListeners.length = 0
  eventListenerCount.value = 0
  hasEventListeners.value = false
  
  // Cancel API subscription if exists
  if (apiSubscription) {
    apiSubscription.unsubscribe()
    apiSubscription = null
    hasApiSubscription.value = false
  }
  
  emitLifecycleEvent(
    'Cleanup Complete', 
    'All resources have been cleaned up properly', 
    'destruction'
  )
}

// 👀 WATCHERS (also part of component lifecycle)

watch(
  () => props.updateTrigger,
  (newValue, oldValue) => {
    emitLifecycleEvent(
      'Prop Changed', 
      `updateTrigger changed from ${oldValue} to ${newValue}`, 
      'update'
    )
  }
)

watch(
  () => props.demoData,
  (newData) => {
    emitLifecycleEvent(
      'Data Changed', 
      'Demo data prop has been updated', 
      'update'
    )
  },
  { deep: true }
)
</script>

<style scoped>
.lifecycle-demo {
  background: #f8f9fa;
  border-radius: 8px;
  padding: 20px;
}

.lifecycle-demo h3 {
  margin-top: 0;
  color: #2c3e50;
  text-align: center;
}

.status-section,
.interactive-section,
.data-section,
.resources-section {
  margin-bottom: 25px;
}

.status-card {
  background: white;
  padding: 15px;
  border-radius: 6px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.status-card h4 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #495057;
}

.status-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 10px;
}

.status-item {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  background: #f8f9fa;
  border-radius: 4px;
}

.status-label {
  font-weight: bold;
}

.status-value {
  color: #007bff;
}

.controls-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.controls-grid button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.2s;
}

.local-btn {
  background: #28a745;
  color: white;
}

.local-btn:hover {
  background: #218838;
}

.fetch-btn {
  background: #007bff;
  color: white;
}

.fetch-btn:hover:not(:disabled) {
  background: #0056b3;
}

.fetch-btn:disabled {
  background: #6c757d;
  cursor: not-allowed;
}

.timer-btn {
  background: #ffc107;
  color: black;
}

.timer-btn:hover:not(:disabled) {
  background: #e0a800;
}

.timer-btn:disabled {
  background: #6c757d;
  color: white;
  cursor: not-allowed;
}

.events-btn {
  background: #6f42c1;
  color: white;
}

.events-btn:hover {
  background: #5a32a3;
}

.data-display {
  background: white;
  padding: 15px;
  border-radius: 6px;
  border: 1px solid #dee2e6;
}

.data-item {
  margin-bottom: 10px;
  font-family: 'Courier New', monospace;
  font-size: 14px;
}

.data-item strong {
  color: #495057;
}

.data-item pre {
  background: #f8f9fa;
  padding: 10px;
  border-radius: 4px;
  margin: 5px 0;
  overflow-x: auto;
}

.resources-list {
  background: white;
  padding: 15px;
  border-radius: 6px;
  border: 1px solid #dee2e6;
}

.resource-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #e9ecef;
}

.resource-item:last-child {
  border-bottom: none;
}

.resource-item span.active {
  color: #28a745;
  font-weight: bold;
}

@media (max-width: 768px) {
  .status-grid,
  .controls-grid {
    grid-template-columns: 1fr;
  }
}
</style>
```

## Understanding Each Lifecycle Hook

### 🌱 **Creation Hooks**

#### `onBeforeMount`
**When:** Just before the component is mounted to the DOM
**Use for:** Last-minute setup before DOM creation
```javascript
onBeforeMount(() => {
  console.log('About to mount - DOM not ready yet')
  // Final preparations before mounting
})
```

#### `onMounted` ⭐ (Most Important)
**When:** Component is mounted and DOM is available
**Use for:** DOM manipulation, API calls, setting up subscriptions
```javascript
onMounted(() => {
  console.log('Component mounted - DOM is ready!')
  // Set up charts, initialize libraries, fetch data
  fetchInitialData()
  setupEventListeners()
})
```

### 🔄 **Update Hooks**

#### `onBeforeUpdate`
**When:** Before component re-renders
**Use for:** Accessing DOM before changes
```javascript
onBeforeUpdate(() => {
  console.log('About to update - current DOM state accessible')
  // Capture scroll position, etc.
})
```

#### `onUpdated`
**When:** After component re-renders
**Use for:** DOM operations after updates
```javascript
onUpdated(() => {
  console.log('Component updated - new DOM state available')
  // Update chart data, scroll to new content
})
```

### 💀 **Destruction Hooks**

#### `onBeforeUnmount` ⭐ (Critical for Cleanup)
**When:** Just before component is destroyed
**Use for:** Cleanup to prevent memory leaks
```javascript
onBeforeUnmount(() => {
  console.log('About to unmount - cleanup time!')
  // Clear timers, remove event listeners, cancel API calls
  clearInterval(timerId)
  window.removeEventListener('scroll', scrollHandler)
})
```

#### `onUnmounted`
**When:** Component is completely destroyed
**Use for:** Final cleanup tasks
```javascript
onUnmounted(() => {
  console.log('Component unmounted and destroyed')
  // Analytics, logging, final notifications
})
```

## Real-World Lifecycle Patterns

### 1. **Data Fetching Pattern**
```javascript
const userData = ref(null)
const isLoading = ref(false)
const error = ref(null)

onMounted(async () => {
  isLoading.value = true
  try {
    userData.value = await fetchUserData()
  } catch (err) {
    error.value = err.message
  } finally {
    isLoading.value = false
  }
})
```

### 2. **Event Listener Pattern**
```javascript
onMounted(() => {
  const handleScroll = () => {
    // Handle scroll
  }
  
  window.addEventListener('scroll', handleScroll)
  
  // Cleanup in beforeUnmount
  onBeforeUnmount(() => {
    window.removeEventListener('scroll', handleScroll)
  })
})
```

### 3. **Timer/Interval Pattern**
```javascript
let intervalId = null

onMounted(() => {
  intervalId = setInterval(() => {
    // Update something every second
    updateTime()
  }, 1000)
})

onBeforeUnmount(() => {
  if (intervalId) {
    clearInterval(intervalId)
  }
})
```

### 4. **Third-Party Library Integration**
```javascript
import Chart from 'chart.js'

let chartInstance = null

onMounted(() => {
  const canvas = document.getElementById('myChart')
  chartInstance = new Chart(canvas, {
    type: 'line',
    data: chartData.value
  })
})

onUpdated(() => {
  if (chartInstance) {
    chartInstance.data = chartData.value
    chartInstance.update()
  }
})

onBeforeUnmount(() => {
  if (chartInstance) {
    chartInstance.destroy()
  }
})
```

### 5. **WebSocket Connection Pattern**
```javascript
let socket = null

onMounted(() => {
  socket = new WebSocket('ws://localhost:8080')
  
  socket.onopen = () => {
    console.log('WebSocket connected')
  }
  
  socket.onmessage = (event) => {
    handleMessage(JSON.parse(event.data))
  }
})

onBeforeUnmount(() => {
  if (socket) {
    socket.close()
  }
})
```

## Advanced Lifecycle Concepts

### 1. **Keep-Alive Hooks**
For components wrapped in `<keep-alive>`:
```javascript
onActivated(() => {
  console.log('Component activated (shown)')
  // Refresh data, resume timers
})

onDeactivated(() => {
  console.log('Component deactivated (hidden)')
  // Pause expensive operations
})
```

### 2. **Error Boundary Hook**
```javascript
onErrorCaptured((error, instance, errorInfo) => {
  console.error('Child component error:', error)
  
  // Log to error service
  logError(error, instance, errorInfo)
  
  // Return false to stop error propagation
  return false
})
```

### 3. **Synchronous Hook Registration**
```javascript
// ✅ Correct: Synchronous registration
onMounted(() => {
  console.log('This works!')
})

// ❌ Wrong: Asynchronous registration
setTimeout(() => {
  onMounted(() => {
    console.log('This won\'t work!')
  })
}, 100)

### 3. **Synchronous Hook Registration**
```javascript
// ✅ Correct: Synchronous registration
onMounted(() => {
  console.log('This works!')
})

// ❌ Wrong: Asynchronous registration
setTimeout(() => {
  onMounted(() => {
    console.log('This won\'t work!')
  })
}, 100)

// ✅ Correct: Hooks in external functions (if called synchronously)
function useDataFetching() {
  onMounted(() => {
    fetchData()
  })
}

// Call synchronously in setup
useDataFetching() // This works!
```

## Common Lifecycle Patterns and Use Cases

### 1. **Dashboard Component with Real-Time Data**
```javascript
const dashboardData = ref(null)
const isConnected = ref(false)
let socket = null
let refreshInterval = null

onMounted(() => {
  // Initial data fetch
  fetchDashboardData()
  
  // Set up real-time updates
  connectWebSocket()
  
  // Periodic refresh as fallback
  refreshInterval = setInterval(fetchDashboardData, 30000)
})

onBeforeUnmount(() => {
  // Clean up all resources
  if (socket) {
    socket.close()
  }
  if (refreshInterval) {
    clearInterval(refreshInterval)
  }
})

const connectWebSocket = () => {
  socket = new WebSocket('ws://api.example.com/dashboard')
  
  socket.onopen = () => {
    isConnected.value = true
  }
  
  socket.onmessage = (event) => {
    dashboardData.value = JSON.parse(event.data)
  }
  
  socket.onclose = () => {
    isConnected.value = false
  }
}
```

### 2. **Form Component with Auto-Save**
```javascript
const formData = ref({})
const hasUnsavedChanges = ref(false)
let saveTimer = null

onMounted(() => {
  // Load saved form data
  loadFormData()
  
  // Set up auto-save
  watch(formData, () => {
    hasUnsavedChanges.value = true
    debouncedSave()
  }, { deep: true })
  
  // Warn before leaving page with unsaved changes
  window.addEventListener('beforeunload', handleBeforeUnload)
})

onBeforeUnmount(() => {
  // Save any pending changes
  if (hasUnsavedChanges.value) {
    saveFormData()
  }
  
  // Clean up
  if (saveTimer) {
    clearTimeout(saveTimer)
  }
  window.removeEventListener('beforeunload', handleBeforeUnload)
})

const debouncedSave = () => {
  if (saveTimer) {
    clearTimeout(saveTimer)
  }
  
  saveTimer = setTimeout(() => {
    saveFormData()
  }, 2000)
}

const handleBeforeUnload = (event) => {
  if (hasUnsavedChanges.value) {
    event.preventDefault()
    event.returnValue = 'You have unsaved changes!'
  }
}
```

### 3. **Chart Component with Responsive Updates**
```javascript
import { Chart } from 'chart.js'

const chartData = ref([])
let chartInstance = null
let resizeObserver = null

onMounted(() => {
  initializeChart()
  setupResizeObserver()
})

onUpdated(() => {
  updateChartData()
})

onBeforeUnmount(() => {
  if (chartInstance) {
    chartInstance.destroy()
  }
  if (resizeObserver) {
    resizeObserver.disconnect()
  }
})

const initializeChart = () => {
  const canvas = document.getElementById('chart')
  chartInstance = new Chart(canvas, {
    type: 'line',
    data: {
      datasets: [{
        data: chartData.value
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false
    }
  })
}

const setupResizeObserver = () => {
  const chartContainer = document.querySelector('.chart-container')
  resizeObserver = new ResizeObserver(() => {
    if (chartInstance) {
      chartInstance.resize()
    }
  })
  resizeObserver.observe(chartContainer)
}

const updateChartData = () => {
  if (chartInstance) {
    chartInstance.data.datasets[0].data = chartData.value
    chartInstance.update()
  }
}
```

### 4. **Video Player Component**
```javascript
const videoRef = ref(null)
const isPlaying = ref(false)
const currentTime = ref(0)
let progressInterval = null

onMounted(() => {
  const video = videoRef.value
  
  // Set up video event listeners
  video.addEventListener('play', () => {
    isPlaying.value = true
    startProgressTracking()
  })
  
  video.addEventListener('pause', () => {
    isPlaying.value = false
    stopProgressTracking()
  })
  
  video.addEventListener('ended', () => {
    isPlaying.value = false
    stopProgressTracking()
  })
  
  // Set up keyboard shortcuts
  document.addEventListener('keydown', handleKeydown)
})

onBeforeUnmount(() => {
  stopProgressTracking()
  document.removeEventListener('keydown', handleKeydown)
})

const startProgressTracking = () => {
  progressInterval = setInterval(() => {
    if (videoRef.value) {
      currentTime.value = videoRef.value.currentTime
    }
  }, 100)
}

const stopProgressTracking = () => {
  if (progressInterval) {
    clearInterval(progressInterval)
    progressInterval = null
  }
}

const handleKeydown = (event) => {
  if (event.code === 'Space') {
    event.preventDefault()
    togglePlayPause()
  }
}
```

## Debugging Lifecycle Hooks

### 1. **Lifecycle Event Logger**
```javascript
const logLifecycleEvent = (hookName, details = {}) => {
  console.log(`🔄 ${hookName}`, {
    timestamp: new Date().toISOString(),
    componentName: 'MyComponent',
    ...details
  })
}

onBeforeMount(() => logLifecycleEvent('onBeforeMount'))
onMounted(() => logLifecycleEvent('onMounted', { domReady: true }))
onBeforeUpdate(() => logLifecycleEvent('onBeforeUpdate'))
onUpdated(() => logLifecycleEvent('onUpdated'))
onBeforeUnmount(() => logLifecycleEvent('onBeforeUnmount'))
onUnmounted(() => logLifecycleEvent('onUnmounted'))
```

### 2. **Performance Monitoring**
```javascript
onBeforeMount(() => {
  console.time('component-mount')
})

onMounted(() => {
  console.timeEnd('component-mount')
  
  // Log component size and performance
  const element = document.querySelector('.my-component')
  console.log('Component dimensions:', {
    width: element.offsetWidth,
    height: element.offsetHeight
  })
})
```

### 3. **Development vs Production Hooks**
```javascript
onMounted(() => {
  if (import.meta.env.DEV) {
    // Development-only code
    console.log('Development mode: Component mounted')
    window.debugComponent = getCurrentInstance()
  }
  
  // Production code
  trackComponentLoad('MyComponent')
})
```

## Testing Lifecycle Hooks

### 1. **Testing Hook Execution**
```javascript
import { mount } from '@vue/test-utils'
import MyComponent from '@/components/MyComponent.vue'

test('calls onMounted hook', () => {
  const mockFetch = jest.fn()
  global.fetch = mockFetch
  
  mount(MyComponent)
  
  expect(mockFetch).toHaveBeenCalled()
})

test('cleans up resources on unmount', () => {
  const clearIntervalSpy = jest.spyOn(global, 'clearInterval')
  const wrapper = mount(MyComponent)
  
  wrapper.unmount()
  
  expect(clearIntervalSpy).toHaveBeenCalled()
})
```

### 2. **Testing Async Operations**
```javascript
test('loads data on mount', async () => {
  const mockData = { id: 1, name: 'Test' }
  global.fetch = jest.fn().mockResolvedValue({
    json: () => Promise.resolve(mockData)
  })
  
  const wrapper = mount(MyComponent)
  
  // Wait for mounted hook and async operations
  await wrapper.vm.$nextTick()
  await new Promise(resolve => setTimeout(resolve, 0))
  
  expect(wrapper.vm.data).toEqual(mockData)
})
```

## Performance Considerations

### 1. **Expensive Operations**
```javascript
onMounted(() => {
  // ❌ Avoid: Heavy computations in lifecycle hooks
  const result = heavyComputation()
  
  // ✅ Better: Use nextTick for non-critical operations
  nextTick(() => {
    heavyComputation()
  })
  
  // ✅ Best: Use setTimeout for very heavy operations
  setTimeout(() => {
    heavyComputation()
  }, 0)
})
```

### 2. **Memory Leak Prevention**
```javascript
const subscriptions = []

onMounted(() => {
  // Keep track of all subscriptions
  subscriptions.push(
    eventBus.on('user-updated', handleUserUpdate),
    websocket.subscribe('notifications', handleNotification),
    store.watch('theme', handleThemeChange)
  )
})

onBeforeUnmount(() => {
  // Clean up all subscriptions
  subscriptions.forEach(unsubscribe => {
    if (typeof unsubscribe === 'function') {
      unsubscribe()
    }
  })
  subscriptions.length = 0
})
```

### 3. **Conditional Hook Logic**
```javascript
onMounted(() => {
  // Only set up expensive features if needed
  if (props.enableRealTimeUpdates) {
    setupWebSocket()
  }
  
  if (props.trackAnalytics) {
    setupAnalytics()
  }
  
  // Always clean up conditionally created resources
  onBeforeUnmount(() => {
    if (websocket) {
      websocket.close()
    }
    if (analytics) {
      analytics.disconnect()
    }
  })
})
```

## Lifecycle Hooks Cheat Sheet

| Hook | When | Common Use Cases |
|------|------|------------------|
| `onBeforeMount` | Before DOM creation | Last-minute setup |
| `onMounted` ⭐ | After DOM creation | API calls, DOM setup, libraries |
| `onBeforeUpdate` | Before re-render | Capture current state |
| `onUpdated` | After re-render | DOM updates, scroll position |
| `onBeforeUnmount` ⭐ | Before destruction | Cleanup timers, events, subscriptions |
| `onUnmounted` | After destruction | Final cleanup, logging |
| `onActivated` | Keep-alive activation | Resume operations |
| `onDeactivated` | Keep-alive deactivation | Pause operations |
| `onErrorCaptured` | Child error | Error handling, logging |

## What You've Mastered! 🏆

- ✅ **Complete lifecycle understanding** - from birth to death of components
- ✅ **Essential hooks** - onMounted, onUpdated, onBeforeUnmount usage
- ✅ **Resource management** - proper cleanup to prevent memory leaks  
- ✅ **Real-world patterns** - data fetching, event handling, third-party integration
- ✅ **Performance optimization** - efficient lifecycle hook usage
- ✅ **Debugging techniques** - monitoring and troubleshooting lifecycle issues
- ✅ **Testing strategies** - validating lifecycle behavior in components

## Best Practices Summary

### 1. **Always Clean Up**
- Clear timers and intervals in `onBeforeUnmount`
- Remove event listeners
- Close WebSocket connections
- Cancel pending API requests

### 2. **Use the Right Hook**
- **Data fetching** → `onMounted`
- **DOM manipulation** → `onMounted` or `onUpdated`
- **Resource cleanup** → `onBeforeUnmount`
- **Third-party setup** → `onMounted`

### 3. **Keep Hooks Simple**
- Extract complex logic into separate functions
- Use async/await for readability
- Handle errors gracefully
- Log important lifecycle events

### 4. **Performance Matters**
- Avoid heavy computations in lifecycle hooks
- Use `nextTick()` for non-critical operations
- Clean up resources to prevent memory leaks
- Monitor performance in development

## What's Next?

In the next project, you'll learn about **slots and content distribution** - Vue's powerful system for creating flexible, reusable components that can accept and display dynamic content. This will teach you how to build truly composable components!

## Quick Reference

### Essential Lifecycle Hooks:
```javascript
import { 
  onBeforeMount, 
  onMounted, 
  onBeforeUpdate, 
  onUpdated, 
  onBeforeUnmount, 
  onUnmounted 
} from 'vue'

// Setup phase
onBeforeMount(() => { /* Final preparations */ })
onMounted(() => { /* DOM ready, fetch data, setup */ })

// Update phase  
onBeforeUpdate(() => { /* Before re-render */ })
onUpdated(() => { /* After re-render */ })

// Cleanup phase
onBeforeUnmount(() => { /* Clean up resources */ })
onUnmounted(() => { /* Final cleanup */ })
```

### Common Cleanup Pattern:
```javascript
let timer = null
let subscription = null

onMounted(() => {
  timer = setInterval(updateData, 1000)
  subscription = eventBus.subscribe('update', handler)
})

onBeforeUnmount(() => {
  if (timer) clearInterval(timer)
  if (subscription) subscription.unsubscribe()
})
```

You now understand the complete Vue component lifecycle and can build robust, leak-free applications! 🌟