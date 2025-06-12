# 09 - Component Communication: Making Components Talk! 💬

## Welcome to Component Orchestration!

You've built individual components, but now it's time to make them work together! Component communication is the heart of Vue applications - it's how parent and child components share data, how user actions flow through your app, and how you build complex interfaces from simple pieces.

## What You'll Master Today
- **Props** - passing data from parent to child components
- **Custom Events** - sending messages from child to parent components  
- **The data flow pattern** - how information moves through your app
- **Prop validation** - ensuring components get the right data
- **Event handling** - responding to user actions across components

## The Component Communication Landscape

### 🔄 The Three Communication Patterns

1. **📤 Parent → Child**: Props (data flows down)
2. **📥 Child → Parent**: Custom Events (events flow up)  
3. **🌐 Any Component → Any Component**: Provide/Inject (for distant relatives)

Think of it like a family conversation - parents share information with their children, children can ask questions or report back to parents, and sometimes grandparents need to communicate with grandchildren directly!

## Complete Component Communication Example

Let's build a practical todo application that demonstrates all communication patterns:

```vue
<!-- App.vue - The Parent Component -->
<template>
  <div id="app">
    <h1>🎯 Vue Todo App - Component Communication Demo</h1>
    
    <!-- Pass data DOWN with props, listen for events UP -->
    <TodoInput 
      :placeholder="inputPlaceholder"
      :disabled="isLoading"
      @add-todo="handleAddTodo"
      @input-change="handleInputChange"
    />
    
    <TodoStats 
      :total="todos.length"
      :completed="completedCount"
      :remaining="remainingCount"
    />
    
    <TodoFilter 
      :current-filter="currentFilter"
      @filter-change="handleFilterChange"
    />
    
    <TodoList 
      :todos="filteredTodos"
      :filter="currentFilter"
      @toggle-todo="handleToggleTodo"
      @delete-todo="handleDeleteTodo"
      @edit-todo="handleEditTodo"
    />
    
    <!-- Show message when no todos -->
    <EmptyState 
      v-if="filteredTodos.length === 0"
      :filter="currentFilter"
      :total-todos="todos.length"
      @clear-filter="currentFilter = 'all'"
    />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TodoInput from './components/TodoInput.vue'
import TodoStats from './components/TodoStats.vue'
import TodoFilter from './components/TodoFilter.vue'
import TodoList from './components/TodoList.vue'
import EmptyState from './components/EmptyState.vue'

// App state
const todos = ref([
  { id: 1, text: 'Learn Vue Props', completed: true },
  { id: 2, text: 'Master Custom Events', completed: false },
  { id: 3, text: 'Build Amazing Apps', completed: false }
])

const currentFilter = ref('all')
const isLoading = ref(false)
const inputPlaceholder = ref('What needs to be done?')

// Computed properties (derived state)
const completedCount = computed(() => 
  todos.value.filter(todo => todo.completed).length
)

const remainingCount = computed(() => 
  todos.value.filter(todo => !todo.completed).length
)

const filteredTodos = computed(() => {
  switch (currentFilter.value) {
    case 'active':
      return todos.value.filter(todo => !todo.completed)
    case 'completed':
      return todos.value.filter(todo => todo.completed)
    default:
      return todos.value
  }
})

// Event handlers (responding to child component events)
const handleAddTodo = (todoText) => {
  if (todoText.trim()) {
    todos.value.push({
      id: Date.now(),
      text: todoText.trim(),
      completed: false
    })
  }
}

const handleToggleTodo = (todoId) => {
  const todo = todos.value.find(t => t.id === todoId)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const handleDeleteTodo = (todoId) => {
  todos.value = todos.value.filter(t => t.id !== todoId)
}

const handleEditTodo = ({ id, newText }) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo && newText.trim()) {
    todo.text = newText.trim()
  }
}

const handleFilterChange = (newFilter) => {
  currentFilter.value = newFilter
}

const handleInputChange = (value) => {
  // Example of reacting to intermediate input changes
  console.log('User is typing:', value)
}
</script>

<style>
#app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
}
</style>
```

Now let's build each child component to demonstrate different communication patterns:

### 1. TodoInput.vue - Props and Events

```vue
<template>
  <div class="todo-input">
    <h3>➕ Add New Todo</h3>
    <div class="input-container">
      <input
        v-model="inputValue"
        :placeholder="placeholder"
        :disabled="disabled"
        @keyup.enter="addTodo"
        @input="handleInput"
        class="todo-input-field"
        maxlength="100"
      >
      <button 
        @click="addTodo" 
        :disabled="disabled || !inputValue.trim()"
        class="add-button"
      >
        Add
      </button>
    </div>
    <p class="character-count">{{ inputValue.length }}/100 characters</p>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

// 📥 PROPS: Receiving data from parent
const props = defineProps({
  placeholder: {
    type: String,
    default: 'Enter todo...'
  },
  disabled: {
    type: Boolean,
    default: false
  },
  maxLength: {
    type: Number,
    default: 100,
    validator: (value) => value > 0 && value <= 500
  }
})

// 📤 EVENTS: Defining what events this component can emit
const emit = defineEmits({
  // Event with validation
  'add-todo': (text) => {
    if (!text || typeof text !== 'string') {
      console.warn('add-todo event requires a non-empty string')
      return false
    }
    return text.trim().length > 0
  },
  // Event without validation
  'input-change': null
})

// Local component state
const inputValue = ref('')

// Methods
const addTodo = () => {
  if (inputValue.value.trim()) {
    // 📤 EMIT: Sending data up to parent
    emit('add-todo', inputValue.value)
    inputValue.value = '' // Clear input after adding
  }
}

const handleInput = () => {
  // 📤 EMIT: Notify parent of input changes
  emit('input-change', inputValue.value)
}

// Watch for external changes (demonstration)
watch(() => props.disabled, (newValue) => {
  if (newValue) {
    console.log('Input has been disabled')
  }
})
</script>

<style scoped>
.todo-input {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  border: 2px solid #e9ecef;
}

.input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.todo-input-field {
  flex: 1;
  padding: 12px;
  border: 2px solid #ced4da;
  border-radius: 4px;
  font-size: 16px;
}

.todo-input-field:focus {
  outline: none;
  border-color: #007bff;
}

.todo-input-field:disabled {
  background-color: #e9ecef;
  cursor: not-allowed;
}

.add-button {
  padding: 12px 24px;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.add-button:hover:not(:disabled) {
  background: #218838;
}

.add-button:disabled {
  background: #6c757d;
  cursor: not-allowed;
}

.character-count {
  margin: 0;
  font-size: 14px;
  color: #6c757d;
  text-align: right;
}
</style>
```

### 2. TodoStats.vue - Props Display

```vue
<template>
  <div class="todo-stats">
    <h3>📊 Todo Statistics</h3>
    <div class="stats-grid">
      <div class="stat-card total">
        <div class="stat-number">{{ total }}</div>
        <div class="stat-label">Total</div>
      </div>
      
      <div class="stat-card completed">
        <div class="stat-number">{{ completed }}</div>
        <div class="stat-label">Completed</div>
      </div>
      
      <div class="stat-card remaining">
        <div class="stat-number">{{ remaining }}</div>
        <div class="stat-label">Remaining</div>
      </div>
      
      <div class="stat-card percentage">
        <div class="stat-number">{{ completionPercentage }}%</div>
        <div class="stat-label">Complete</div>
      </div>
    </div>
    
    <!-- Progress bar -->
    <div class="progress-container">
      <div 
        class="progress-bar" 
        :style="{ width: completionPercentage + '%' }"
      ></div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

// 📥 PROPS: All the data we need from parent
const props = defineProps({
  total: {
    type: Number,
    required: true,
    validator: (value) => value >= 0
  },
  completed: {
    type: Number,
    required: true,
    validator: (value) => value >= 0
  },
  remaining: {
    type: Number,
    required: true,
    validator: (value) => value >= 0
  }
})

// 🧮 COMPUTED: Derived values from props
const completionPercentage = computed(() => {
  if (props.total === 0) return 0
  return Math.round((props.completed / props.total) * 100)
})
</script>

<style scoped>
.todo-stats {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.stat-card {
  text-align: center;
  padding: 15px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  backdrop-filter: blur(10px);
}

.stat-number {
  font-size: 2em;
  font-weight: bold;
  margin-bottom: 5px;
}

.stat-label {
  font-size: 0.9em;
  opacity: 0.9;
}

.progress-container {
  background: rgba(255, 255, 255, 0.2);
  height: 8px;
  border-radius: 4px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  background: #28a745;
  border-radius: 4px;
  transition: width 0.3s ease;
}
</style>
```

### 3. TodoFilter.vue - Props and Events

```vue
<template>
  <div class="todo-filter">
    <h3>🔍 Filter Todos</h3>
    <div class="filter-buttons">
      <button
        v-for="filter in filters"
        :key="filter.value"
        @click="changeFilter(filter.value)"
        :class="['filter-btn', { active: currentFilter === filter.value }]"
      >
        {{ filter.label }}
        <span class="filter-count" v-if="filter.count !== undefined">
          ({{ filter.count }})
        </span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

// 📥 PROPS: Current filter and data for counts
const props = defineProps({
  currentFilter: {
    type: String,
    required: true,
    validator: (value) => ['all', 'active', 'completed'].includes(value)
  },
  totalCount: {
    type: Number,
    default: 0
  },
  activeCount: {
    type: Number,
    default: 0
  },
  completedCount: {
    type: Number,
    default: 0
  }
})

// 📤 EVENTS: What this component can emit
const emit = defineEmits({
  'filter-change': (filter) => {
    return ['all', 'active', 'completed'].includes(filter)
  }
})

// 🧮 COMPUTED: Filter options with counts
const filters = computed(() => [
  { value: 'all', label: 'All', count: props.totalCount },
  { value: 'active', label: 'Active', count: props.activeCount },
  { value: 'completed', label: 'Completed', count: props.completedCount }
])

// Methods
const changeFilter = (newFilter) => {
  if (newFilter !== props.currentFilter) {
    // 📤 EMIT: Tell parent about filter change
    emit('filter-change', newFilter)
  }
}
</script>

<style scoped>
.todo-filter {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  border: 2px solid #e9ecef;
}

.filter-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.filter-btn {
  padding: 10px 20px;
  border: 2px solid #007bff;
  background: white;
  color: #007bff;
  border-radius: 20px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.2s;
}

.filter-btn:hover {
  background: #e7f3ff;
}

.filter-btn.active {
  background: #007bff;
  color: white;
}

.filter-count {
  margin-left: 8px;
  font-size: 0.9em;
  opacity: 0.8;
}
</style>
```

### 4. TodoList.vue and TodoItem.vue - Complex Communication

```vue
<!-- TodoList.vue -->
<template>
  <div class="todo-list">
    <h3>📝 Todo List</h3>
    <div class="todos-container">
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @toggle="$emit('toggle-todo', $event)"
        @delete="$emit('delete-todo', $event)"
        @edit="$emit('edit-todo', $event)"
      />
    </div>
  </div>
</template>

<script setup>
import TodoItem from './TodoItem.vue'

// 📥 PROPS: List of todos to display
defineProps({
  todos: {
    type: Array,
    required: true,
    validator: (todos) => {
      return todos.every(todo => 
        todo && 
        typeof todo.id !== 'undefined' && 
        typeof todo.text === 'string' &&
        typeof todo.completed === 'boolean'
      )
    }
  },
  filter: {
    type: String,
    default: 'all'
  }
})

// 📤 EVENTS: Pass through events from child components
defineEmits(['toggle-todo', 'delete-todo', 'edit-todo'])
</script>

<style scoped>
.todo-list {
  background: white;
  border: 2px solid #e9ecef;
  border-radius: 8px;
  overflow: hidden;
}

.todo-list h3 {
  background: #e9ecef;
  margin: 0;
  padding: 15px 20px;
  border-bottom: 2px solid #dee2e6;
}

.todos-container {
  max-height: 400px;
  overflow-y: auto;
}
</style>
```

```vue
<!-- TodoItem.vue -->
<template>
  <div :class="['todo-item', { completed: todo.completed }]">
    <div class="todo-content">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="toggleComplete"
        class="todo-checkbox"
      >
      
      <span
        v-if="!isEditing"
        @dblclick="startEditing"
        class="todo-text"
      >
        {{ todo.text }}
      </span>
      
      <input
        v-else
        v-model="editText"
        @keyup.enter="saveEdit"
        @keyup.esc="cancelEdit"
        @blur="saveEdit"
        ref="editInput"
        class="todo-edit-input"
      >
    </div>
    
    <div class="todo-actions">
      <button
        v-if="!isEditing"
        @click="startEditing"
        class="edit-btn"
        title="Edit todo"
      >
        ✏️
      </button>
      
      <button
        @click="deleteTodo"
        class="delete-btn"
        title="Delete todo"
      >
        🗑️
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick } from 'vue'

// 📥 PROPS: Individual todo item
const props = defineProps({
  todo: {
    type: Object,
    required: true,
    validator: (todo) => {
      return todo &&
        typeof todo.id !== 'undefined' &&
        typeof todo.text === 'string' &&
        typeof todo.completed === 'boolean'
    }
  }
})

// 📤 EVENTS: Actions this item can perform
const emit = defineEmits({
  toggle: (id) => typeof id !== 'undefined',
  delete: (id) => typeof id !== 'undefined',
  edit: (payload) => {
    return payload &&
      typeof payload.id !== 'undefined' &&
      typeof payload.newText === 'string'
  }
})

// Local state for editing
const isEditing = ref(false)
const editText = ref('')
const editInput = ref(null)

// Methods
const toggleComplete = () => {
  // 📤 EMIT: Tell parent to toggle this todo
  emit('toggle', props.todo.id)
}

const deleteTodo = () => {
  // 📤 EMIT: Tell parent to delete this todo
  emit('delete', props.todo.id)
}

const startEditing = async () => {
  isEditing.value = true
  editText.value = props.todo.text
  
  // Focus the input after it's rendered
  await nextTick()
  editInput.value?.focus()
}

const saveEdit = () => {
  if (editText.value.trim() && editText.value !== props.todo.text) {
    // 📤 EMIT: Tell parent about the edit
    emit('edit', {
      id: props.todo.id,
      newText: editText.value
    })
  }
  cancelEdit()
}

const cancelEdit = () => {
  isEditing.value = false
  editText.value = ''
}
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  border-bottom: 1px solid #e9ecef;
  transition: background-color 0.2s;
}

.todo-item:hover {
  background-color: #f8f9fa;
}

.todo-item.completed {
  opacity: 0.6;
}

.todo-content {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 12px;
}

.todo-checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.todo-text {
  flex: 1;
  cursor: pointer;
  user-select: none;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
}

.todo-edit-input {
  flex: 1;
  padding: 8px;
  border: 2px solid #007bff;
  border-radius: 4px;
  font-size: inherit;
}

.todo-actions {
  display: flex;
  gap: 8px;
}

.edit-btn,
.delete-btn {
  padding: 8px;
  border: none;
  background: none;
  cursor: pointer;
  border-radius: 4px;
  font-size: 16px;
  transition: background-color 0.2s;
}

.edit-btn:hover {
  background-color: #e7f3ff;
}

.delete-btn:hover {
  background-color: #ffebee;
}
</style>
```

### 5. EmptyState.vue - Conditional Display

```vue
<template>
  <div class="empty-state">
    <div class="empty-icon">{{ emptyIcon }}</div>
    <h3>{{ emptyTitle }}</h3>
    <p>{{ emptyMessage }}</p>
    
    <button
      v-if="filter !== 'all' && totalTodos > 0"
      @click="clearFilter"
      class="clear-filter-btn"
    >
      Show All Todos
    </button>
  </div>
</template>

<script setup>
import { computed } from 'vue'

// 📥 PROPS: Current state information
const props = defineProps({
  filter: {
    type: String,
    required: true
  },
  totalTodos: {
    type: Number,
    default: 0
  }
})

// 📤 EVENTS: Actions this component can trigger
const emit = defineEmits(['clear-filter'])

// 🧮 COMPUTED: Dynamic content based on state
const emptyIcon = computed(() => {
  if (props.totalTodos === 0) return '📝'
  if (props.filter === 'active') return '✅'
  if (props.filter === 'completed') return '⏳'
  return '🔍'
})

const emptyTitle = computed(() => {
  if (props.totalTodos === 0) return 'No todos yet!'
  if (props.filter === 'active') return 'All done!'
  if (props.filter === 'completed') return 'Nothing completed yet'
  return 'No todos found'
})

const emptyMessage = computed(() => {
  if (props.totalTodos === 0) return 'Add your first todo above to get started.'
  if (props.filter === 'active') return 'You\'ve completed all your todos. Great job!'
  if (props.filter === 'completed') return 'Complete some todos to see them here.'
  return 'Try a different filter or add some todos.'
})

// Methods
const clearFilter = () => {
  // 📤 EMIT: Tell parent to clear the filter
  emit('clear-filter')
}
</script>

<style scoped>
.empty-state {
  text-align: center;
  padding: 40px 20px;
  color: #6c757d;
  background: #f8f9fa;
  border: 2px dashed #dee2e6;
  border-radius: 8px;
  margin-top: 20px;
}

.empty-icon {
  font-size: 4em;
  margin-bottom: 20px;
}

.empty-state h3 {
  margin-bottom: 10px;
  color: #495057;
}

.empty-state p {
  margin-bottom: 20px;
  font-size: 1.1em;
}

.clear-filter-btn {
  padding: 12px 24px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.2s;
}

.clear-filter-btn:hover {
  background: #0056b3;
}
</style>
```

## Understanding Component Communication Patterns

### 📤 Props: Parent to Child Data Flow

**Props are like function parameters** - they pass data down from parent to child components:

```javascript
// Parent passes data down
<TodoInput :placeholder="inputPlaceholder" :disabled="isLoading" />

// Child receives and validates
const props = defineProps({
  placeholder: {
    type: String,
    default: 'Enter todo...'
  },
  disabled: {
    type: Boolean,
    default: false
  }
})
```

**Key prop concepts:**
- **One-way data flow** - data flows down only
- **Reactive updates** - when parent data changes, child updates automatically
- **Prop validation** - ensures components get the right type of data
- **Default values** - fallbacks when props aren't provided

### 📥 Custom Events: Child to Parent Communication

**Events are like phone calls** - children call parents to report something:

```javascript
// Child defines what events it can emit
const emit = defineEmits({
  'add-todo': (text) => typeof text === 'string' && text.trim().length > 0
})

// Child emits an event with data
const addTodo = () => {
  emit('add-todo', inputValue.value)
}

// Parent listens and responds
<TodoInput @add-todo="handleAddTodo" />
```

**Key event concepts:**
- **Event declaration** - `defineEmits()` documents what events component can send
- **Event validation** - ensure events are sent with correct data
- **Event handling** - parent responds to child events
- **Data payload** - events can carry data from child to parent

### 🌐 Provide/Inject: Skip the Chain

**For deeply nested components** that need to communicate without passing props through every level:

```javascript
// Grandparent provides data
provide('theme', {
  primaryColor: '#007bff',
  backgroundColor: '#f8f9fa'
})

// Grandchild injects data (skipping parent)
const theme = inject('theme')
```

## Prop Validation Deep Dive

Vue provides powerful prop validation to catch bugs early:

```javascript
defineProps({
  // Basic type checking
  title: String,
  likes: Number,
  isPublished: Boolean,
  
  // Multiple possible types
  propB: [String, Number],
  
  // Required prop
  propC: {
    type: String,
    required: true
  },
  
  // With default value
  propD: {
    type: Number,
    default: 100
  },
  
  // Object/array defaults from factory function
  propE: {
    type: Object,
    default() {
      return { message: 'hello' }
    }
  },
  
  // Custom validator
  propF: {
    validator(value) {
      return ['success', 'warning', 'danger'].includes(value)
    }
  }
})
```

## Event Validation and Best Practices

```javascript
defineEmits({
  // No validation
  click: null,
  
  // With validation
  'user-login': (payload) => {
    // Return true if valid, false if invalid
    return payload && 
           typeof payload.email === 'string' &&
           typeof payload.password === 'string'
  },
  
  // Complex validation
  'form-submit': (formData) => {
    const requiredFields = ['name', 'email']
    return requiredFields.every(field => 
      formData[field] && formData[field].trim().length > 0
    )
  }
})
```

## Common Communication Patterns

### 1. **Form Components**
```javascript
// Parent
<UserForm 
  :initial-data="userData"
  :is-loading="isSubmitting"
  @form-submit="handleSubmit"
  @form-cancel="handleCancel"
/>

// Child
const emit = defineEmits(['form-submit', 'form-cancel'])
const submitForm = () => {
  emit('form-submit', formData.value)
}
```

### 2. **Modal Components**
```javascript
// Parent
<Modal 
  :is-open="showModal"
  :title="modalTitle"
  @close="showModal = false"
  @confirm="handleConfirm"
/>

// Child
const props = defineProps(['isOpen', 'title'])
const emit = defineEmits(['close', 'confirm'])
```

### 3. **List and Item Components**
```javascript
// Parent (List)
<TodoItem
  v-for="todo in todos"
  :key="todo.id"
  :todo="todo"
  @update="updateTodo"
  @delete="deleteTodo"
/>

// Child (Item)
const props = defineProps(['todo'])
const emit = defineEmits(['update', 'delete'])
```

## Debugging Component Communication

### 1. **Vue DevTools**
- See prop values in real-time
- Track event emissions
- Inspect component relationships

### 2. **Console Logging**
```javascript
// In child component
watch(() => props.someValue, (newValue, oldValue) => {
  console.log('Prop changed:', { newValue, oldValue })
})

// In parent component
const handleChildEvent = (payload) => {
  console.log('Received event:', payload)
}
```

### 3. **Prop Validation Warnings**
Vue will warn you in the console when:
- Props are missing or wrong type
- Events are emitted incorrectly
- Required props aren't provided

## Performance Considerations

### 1. **Prop Reactivity**
```javascript
// ✅ Good: Reactive prop usage
const computedValue = computed(() => props.data.length)

// ❌ Avoid: Destructuring reactive props (loses reactivity in Vue 3.4 and below)
const { data } = defineProps(['data'])
// Use data directly, but it won't be reactive to changes

// ✅ Better: Keep props object
const props = defineProps(['data'])
const computedValue = computed(() => props.data.length)
```

### 2. **Event Optimization**
```javascript
// ✅ Good: Specific event listeners
<button @click="handleSpecificAction">Click me</button>

// ❌ Avoid: Generic event handlers that check event type
<button @click="handleGenericEvent('click', $event)">Click me</button>
```

### 3. **Prop Validation Performance**
```javascript
// ✅ Good for development: Detailed validation
defineProps({
  users: {
    type: Array,
    validator: (users) => users.every(user => user.id && user.name)
  }
})

// Note: Prop validation only runs in development mode
// Production builds automatically remove these checks
```

## Troubleshooting Common Issues

### 1. **Props Not Updating**
```javascript
// Problem: Child component not seeing prop changes
// Solution: Check prop name casing

// ❌ Wrong: Mismatched casing
// Parent: <Child user-name="John" />
// Child: defineProps(['userName']) // Won't work!

// ✅ Correct: Consistent casing
// Parent: <Child user-name="John" />
// Child: defineProps(['userName']) // Use camelCase in JavaScript
// Vue automatically converts kebab-case to camelCase
```

### 2. **Events Not Firing**
```javascript
// Problem: Parent not receiving child events
// Solution: Check event names and listeners

// ❌ Wrong: Event name mismatch
// Child: emit('userLogin', data)
// Parent: <Child @user-login="handle" /> // Won't work!

// ✅ Correct: Consistent naming
// Child: emit('user-login', data) // Use kebab-case for events
// Parent: <Child @user-login="handle" />
```

### 3. **Prop Validation Errors**
```javascript
// Problem: Console warnings about prop types
// Solution: Ensure data types match

// ❌ Wrong: Passing wrong type
// Parent: <Child count="5" /> // String instead of number
// Child: defineProps({ count: Number })

// ✅ Correct: Use v-bind for non-strings
// Parent: <Child :count="5" /> // Number
// Child: defineProps({ count: Number })
```

## Advanced Communication Patterns

### 1. **Multiple Event Arguments**
```javascript
// Child can emit multiple arguments
const emit = defineEmits(['complex-event'])

const handleComplexAction = () => {
  emit('complex-event', 
    'first argument',
    { id: 1, name: 'John' },
    ['tag1', 'tag2']
  )
}

// Parent receives all arguments
const handleComplexEvent = (arg1, arg2, arg3) => {
  console.log('First:', arg1)    // 'first argument'
  console.log('Second:', arg2)   // { id: 1, name: 'John' }
  console.log('Third:', arg3)    // ['tag1', 'tag2']
}
```

### 2. **Event Chaining Through Components**
```javascript
// TodoList.vue - Middle component that passes events through
<template>
  <TodoItem
    v-for="todo in todos"
    :key="todo.id"
    :todo="todo"
    @toggle="$emit('toggle-todo', $event)"
    @delete="$emit('delete-todo', $event)"
  />
</template>

<script setup>
// Pass-through events: child events → parent events
defineEmits(['toggle-todo', 'delete-todo'])
</script>
```

### 3. **Conditional Event Emission**
```javascript
// Only emit events under certain conditions
const handleSubmit = () => {
  if (isValid.value) {
    emit('form-submit', formData.value)
  } else {
    emit('form-error', validationErrors.value)
  }
}
```

## Real-World Communication Examples

### 1. **Shopping Cart Component**
```javascript
// CartItem.vue
const props = defineProps({
  item: Object,
  readonly: Boolean
})

const emit = defineEmits({
  'update-quantity': (payload) => {
    return payload && 
           typeof payload.id !== 'undefined' &&
           typeof payload.quantity === 'number' &&
           payload.quantity >= 0
  },
  'remove-item': (id) => typeof id !== 'undefined'
})

// Cart.vue
<CartItem
  v-for="item in cartItems"
  :key="item.id"
  :item="item"
  :readonly="isCheckingOut"
  @update-quantity="updateQuantity"
  @remove-item="removeFromCart"
/>
```

### 2. **Data Table Component**
```javascript
// DataTable.vue
const props = defineProps({
  data: Array,
  columns: Array,
  sortable: Boolean,
  selectable: Boolean
})

const emit = defineEmits([
  'sort-change',
  'row-select',
  'row-click',
  'page-change'
])

// TableRow.vue
<tr 
  @click="$emit('row-click', row)"
  :class="{ selected: isSelected }"
>
  <td v-if="selectable">
    <input 
      type="checkbox" 
      :checked="isSelected"
      @change="$emit('row-select', row.id, $event.target.checked)"
    >
  </td>
  <td v-for="column in columns" :key="column.key">
    {{ row[column.key] }}
  </td>
</tr>
```

### 3. **Form Wizard Component**
```javascript
// FormWizard.vue
const props = defineProps({
  steps: Array,
  currentStep: Number
})

const emit = defineEmits({
  'step-change': (stepIndex) => {
    return typeof stepIndex === 'number' && 
           stepIndex >= 0 && 
           stepIndex < props.steps.length
  },
  'wizard-complete': (formData) => typeof formData === 'object',
  'wizard-cancel': null
})

// WizardStep.vue
const props = defineProps({
  title: String,
  isActive: Boolean,
  isCompleted: Boolean,
  isValid: Boolean
})

const emit = defineEmits(['next', 'previous', 'complete'])
```

## Testing Component Communication

### 1. **Testing Props**
```javascript
// Component test example
import { mount } from '@vue/test-utils'
import TodoItem from '@/components/TodoItem.vue'

test('displays todo text from prop', () => {
  const todo = { id: 1, text: 'Test todo', completed: false }
  const wrapper = mount(TodoItem, {
    props: { todo }
  })
  
  expect(wrapper.text()).toContain('Test todo')
})

test('validates required props', () => {
  // Vue will warn in console if required props are missing
  const wrapper = mount(TodoItem) // Missing required 'todo' prop
  // Check console for validation warnings
})
```

### 2. **Testing Events**
```javascript
test('emits delete event when delete button clicked', async () => {
  const todo = { id: 1, text: 'Test todo', completed: false }
  const wrapper = mount(TodoItem, {
    props: { todo }
  })
  
  await wrapper.find('.delete-btn').trigger('click')
  
  expect(wrapper.emitted('delete')).toBeTruthy()
  expect(wrapper.emitted('delete')[0]).toEqual([1]) // Event payload
})
```

## What You've Mastered! 🏆

- ✅ **Props system** - passing data down from parent to child
- ✅ **Custom events** - sending messages up from child to parent
- ✅ **Prop validation** - ensuring components get correct data types
- ✅ **Event validation** - verifying events are emitted correctly
- ✅ **Communication patterns** - real-world component interaction
- ✅ **Performance optimization** - efficient prop and event usage
- ✅ **Debugging techniques** - troubleshooting communication issues
- ✅ **Advanced patterns** - complex component orchestration

## Key Principles to Remember

### 1. **Data Down, Events Up**
- **Props** flow data from parent to child
- **Events** send information from child to parent
- This creates predictable, maintainable data flow

### 2. **Single Responsibility**
- Each component should have a clear, focused purpose
- Communication should be explicit and well-defined
- Avoid tight coupling between components

### 3. **Validation is Your Friend**
- Always validate props in shared/reusable components
- Use event validation to catch bugs early
- Provide meaningful defaults for optional props

### 4. **Think in Terms of Contracts**
- Props define what data a component needs
- Events define what actions a component can perform
- Both serve as documentation for other developers

## What's Next?

In the next project, you'll learn about **computed properties and watchers** - powerful reactive programming concepts that help you create derived state and respond to data changes efficiently. These will take your component logic to the next level!

## Quick Reference

### Essential Patterns:
```javascript
// Props (Parent → Child)
<ChildComponent :data="parentData" :config="settings" />

// Events (Child → Parent)  
<ChildComponent @action="handleAction" @change="handleChange" />

// Prop Declaration
defineProps({
  title: String,
  required: { type: Boolean, default: false }
})

// Event Declaration
defineEmits({
  'action': (payload) => typeof payload === 'object'
})

// Event Emission
emit('action', { id: 1, type: 'update' })
```

### Common Prop Types:
- **String, Number, Boolean** - Primitive types
- **Array, Object** - Complex types (provide defaults as functions)
- **Function** - Method props for callbacks
- **[String, Number]** - Multiple allowed types

### Event Best Practices:
- Use **kebab-case** for event names
- Include **validation** for complex events
- Provide **meaningful event names** that describe the action
- Keep **event payloads** focused and minimal

You now have the foundation for building complex, interactive Vue applications with proper component communication! 🌟