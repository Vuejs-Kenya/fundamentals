# 04 - Show, Hide & Lists - Dynamic Content Magic! ✨

## Welcome to Dynamic Content!
Now you're going to learn one of Vue's most powerful features - showing and hiding content based on conditions, and displaying lists of data that can change! This is where your apps start feeling truly dynamic and responsive.

## What You'll Master Today
- **Conditional rendering** - show/hide elements based on data
- **The difference between `v-if` and `v-show`** - and when to use each
- **List rendering** - display dynamic lists that automatically update
- **The importance of `key` attributes** for list performance
- **Real-world patterns** you'll use in every Vue app

## Understanding Conditional Rendering

Imagine you want to show a "Welcome" message only if a user is logged in, or display an error only if something went wrong. In regular HTML, you'd need complex JavaScript. In Vue, it's as simple as adding `v-if`!

## Your Dynamic Content Playground

Let's build a comprehensive app that demonstrates all these concepts:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Dynamic Content Playground</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            max-width: 800px; 
            margin: 20px auto; 
            padding: 20px; 
        }
        .section { 
            margin: 30px 0; 
            padding: 20px; 
            border: 1px solid #ddd; 
            border-radius: 8px; 
            background: #f9f9f9; 
        }
        .alert { 
            padding: 15px; 
            margin: 10px 0; 
            border-radius: 5px; 
            font-weight: bold; 
        }
        .success { background: #d4edda; color: #155724; border: 1px solid #c3e6cb; }
        .warning { background: #fff3cd; color: #856404; border: 1px solid #ffeaa7; }
        .error { background: #f8d7da; color: #721c24; border: 1px solid #f5c6cb; }
        .info { background: #d1ecf1; color: #0c5460; border: 1px solid #bee5eb; }
        .todo-item { 
            padding: 10px; 
            margin: 5px 0; 
            border: 1px solid #ddd; 
            border-radius: 4px; 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
        }
        .todo-item.completed { 
            background: #f0f0f0; 
            text-decoration: line-through; 
            opacity: 0.7; 
        }
        .user-card { 
            border: 1px solid #ddd; 
            border-radius: 8px; 
            padding: 15px; 
            margin: 10px; 
            display: inline-block; 
            width: 200px; 
            text-align: center; 
        }
        .online { border-color: #28a745; background: #f8fff9; }
        .offline { border-color: #dc3545; background: #fff8f8; }
        button { 
            padding: 8px 16px; 
            margin: 5px; 
            border: none; 
            border-radius: 4px; 
            cursor: pointer; 
            font-size: 14px; 
        }
        .btn-primary { background: #007bff; color: white; }
        .btn-success { background: #28a745; color: white; }
        .btn-danger { background: #dc3545; color: white; }
        .btn-warning { background: #ffc107; color: black; }
        button:hover { opacity: 0.8; }
        input, select { 
            padding: 8px; 
            margin: 5px; 
            border: 1px solid #ddd; 
            border-radius: 4px; 
        }
        .stats { 
            display: flex; 
            gap: 20px; 
            margin: 20px 0; 
        }
        .stat-card { 
            padding: 15px; 
            border-radius: 8px; 
            text-align: center; 
            flex: 1; 
        }
        .hidden { display: none; }
        .fade-toggle { transition: opacity 0.3s; }
    </style>
</head>
<body>
    <!-- Include Vue -->
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

    <div id="app">
        <h1>🎭 Dynamic Content Playground</h1>

        <!-- Section 1: Basic Conditional Rendering -->
        <div class="section">
            <h2>🎯 Basic Show/Hide with v-if</h2>
            
            <button @click="isLoggedIn = !isLoggedIn" class="btn-primary">
                {{ isLoggedIn ? 'Logout' : 'Login' }}
            </button>
            
            <!-- v-if: Completely removes/adds element -->
            <div v-if="isLoggedIn" class="alert success">
                ✅ Welcome back! You are logged in.
            </div>
            <div v-else class="alert warning">
                ⚠️ Please log in to access your account.
            </div>

            <!-- v-show: Always in DOM, just hidden -->
            <p>
                <strong>v-show example:</strong>
                <span v-show="isLoggedIn" style="color: green;">🟢 Online</span>
                <span v-show="!isLoggedIn" style="color: red;">🔴 Offline</span>
            </p>
        </div>

        <!-- Section 2: Multiple Conditions with v-else-if -->
        <div class="section">
            <h2>🚦 Multiple Conditions with v-else-if</h2>
            
            <select v-model="userType">
                <option value="">Select user type</option>
                <option value="admin">Admin</option>
                <option value="moderator">Moderator</option>
                <option value="user">Regular User</option>
                <option value="guest">Guest</option>
            </select>

            <div v-if="userType === 'admin'" class="alert success">
                👑 Admin Access: You can manage everything!
            </div>
            <div v-else-if="userType === 'moderator'" class="alert info">
                🛡️ Moderator Access: You can manage content.
            </div>
            <div v-else-if="userType === 'user'" class="alert info">
                👤 User Access: You can view and edit your content.
            </div>
            <div v-else-if="userType === 'guest'" class="alert warning">
                👻 Guest Access: Limited viewing permissions.
            </div>
            <div v-else class="alert error">
                ❌ Please select a user type above.
            </div>
        </div>

        <!-- Section 3: v-if vs v-show Comparison -->
        <div class="section">
            <h2>⚖️ v-if vs v-show: See the Difference!</h2>
            
            <button @click="showComparison = !showComparison" class="btn-warning">
                Toggle Comparison
            </button>
            
            <div style="display: flex; gap: 20px; margin-top: 15px;">
                <div style="flex: 1;">
                    <h4>Using v-if (removes from DOM):</h4>
                    <div v-if="showComparison" class="alert info">
                        🗑️ I get completely removed from the DOM when hidden!
                    </div>
                </div>
                
                <div style="flex: 1;">
                    <h4>Using v-show (hidden with CSS):</h4>
                    <div v-show="showComparison" class="alert info">
                        👻 I stay in the DOM but become invisible!
                    </div>
                </div>
            </div>
            
            <p><small>💡 <strong>Tip:</strong> Open browser dev tools and watch the DOM when you toggle!</small></p>
        </div>

        <!-- Section 4: List Rendering with v-for -->
        <div class="section">
            <h2>📝 Dynamic Lists with v-for</h2>
            
            <!-- Simple Todo List -->
            <h3>Simple Todo List</h3>
            <div>
                <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new todo...">
                <button @click="addTodo" class="btn-success">Add Todo</button>
            </div>
            
            <div v-if="todos.length === 0" class="alert info">
                📝 No todos yet. Add one above!
            </div>
            
            <div v-else>
                <div v-for="todo in todos" :key="todo.id" class="todo-item" :class="{ completed: todo.completed }">
                    <span>{{ todo.text }}</span>
                    <div>
                        <button @click="toggleTodo(todo.id)" class="btn-warning">
                            {{ todo.completed ? 'Undo' : 'Done' }}
                        </button>
                        <button @click="removeTodo(todo.id)" class="btn-danger">Delete</button>
                    </div>
                </div>
            </div>

            <!-- List Statistics -->
            <div v-if="todos.length > 0" class="stats">
                <div class="stat-card" style="background: #e7f3ff;">
                    <strong>{{ totalTodos }}</strong><br>
                    Total Todos
                </div>
                <div class="stat-card" style="background: #e8f5e8;">
                    <strong>{{ completedTodos }}</strong><br>
                    Completed
                </div>
                <div class="stat-card" style="background: #fff3e0;">
                    <strong>{{ remainingTodos }}</strong><br>
                    Remaining
                </div>
            </div>
        </div>

        <!-- Section 5: Complex List with Objects -->
        <div class="section">
            <h2>👥 User List with Complex Data</h2>
            
            <div>
                <button @click="addRandomUser" class="btn-success">Add Random User</button>
                <button @click="toggleAllOnline" class="btn-primary">Toggle All Online/Offline</button>
                <select v-model="filterStatus">
                    <option value="all">Show All</option>
                    <option value="online">Online Only</option>
                    <option value="offline">Offline Only</option>
                </select>
            </div>

            <div v-if="filteredUsers.length === 0" class="alert warning">
                No users match the current filter.
            </div>

            <div v-for="user in filteredUsers" :key="user.id" class="user-card" :class="{ online: user.isOnline, offline: !user.isOnline }">
                <h4>{{ user.name }}</h4>
                <p>{{ user.email }}</p>
                <p><strong>Role:</strong> {{ user.role }}</p>
                <p>
                    Status: 
                    <span :style="{ color: user.isOnline ? 'green' : 'red' }">
                        {{ user.isOnline ? '🟢 Online' : '🔴 Offline' }}
                    </span>
                </p>
                <button @click="toggleUserStatus(user.id)" class="btn-warning">
                    {{ user.isOnline ? 'Set Offline' : 'Set Online' }}
                </button>
                <button @click="removeUser(user.id)" class="btn-danger">Remove</button>
            </div>
        </div>

        <!-- Section 6: Nested Lists -->
        <div class="section">
            <h2>🏢 Nested Lists: Departments & Employees</h2>
            
            <div v-for="department in departments" :key="department.id" style="margin: 20px 0; padding: 15px; border: 1px solid #ccc; border-radius: 8px;">
                <h3>{{ department.name }} ({{ department.employees.length }} employees)</h3>
                
                <div v-if="department.employees.length === 0" class="alert info">
                    No employees in this department yet.
                </div>
                
                <div v-else>
                    <ul>
                        <li v-for="employee in department.employees" :key="employee.id">
                            <strong>{{ employee.name }}</strong> - {{ employee.position }}
                            <span v-if="employee.isManager" style="color: gold;">👑 Manager</span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>

    <script>
        const { createApp, ref, computed } = Vue

        createApp({
            setup() {
                // Basic conditional data
                const isLoggedIn = ref(false)
                const userType = ref('')
                const showComparison = ref(true)
                
                // Todo list data
                const newTodo = ref('')
                const todos = ref([
                    { id: 1, text: 'Learn Vue.js', completed: false },
                    { id: 2, text: 'Build awesome apps', completed: false },
                    { id: 3, text: 'Master conditional rendering', completed: true }
                ])
                
                // User list data
                const users = ref([
                    { id: 1, name: 'Alice Johnson', email: 'alice@example.com', role: 'Admin', isOnline: true },
                    { id: 2, name: 'Bob Smith', email: 'bob@example.com', role: 'User', isOnline: false },
                    { id: 3, name: 'Carol Brown', email: 'carol@example.com', role: 'Moderator', isOnline: true }
                ])
                const filterStatus = ref('all')
                
                // Departments data (nested lists)
                const departments = ref([
                    {
                        id: 1,
                        name: 'Engineering',
                        employees: [
                            { id: 1, name: 'John Doe', position: 'Senior Developer', isManager: true },
                            { id: 2, name: 'Jane Smith', position: 'Frontend Developer', isManager: false },
                            { id: 3, name: 'Mike Johnson', position: 'Backend Developer', isManager: false }
                        ]
                    },
                    {
                        id: 2,
                        name: 'Marketing',
                        employees: [
                            { id: 4, name: 'Sarah Wilson', position: 'Marketing Manager', isManager: true },
                            { id: 5, name: 'Tom Brown', position: 'Content Creator', isManager: false }
                        ]
                    },
                    {
                        id: 3,
                        name: 'Sales',
                        employees: []
                    }
                ])
                
                // Computed properties for todo statistics
                const totalTodos = computed(() => todos.value.length)
                const completedTodos = computed(() => todos.value.filter(todo => todo.completed).length)
                const remainingTodos = computed(() => todos.value.filter(todo => !todo.completed).length)
                
                // Computed property for filtered users
                const filteredUsers = computed(() => {
                    if (filterStatus.value === 'all') return users.value
                    if (filterStatus.value === 'online') return users.value.filter(user => user.isOnline)
                    if (filterStatus.value === 'offline') return users.value.filter(user => !user.isOnline)
                    return users.value
                })
                
                // Todo methods
                const addTodo = () => {
                    if (newTodo.value.trim()) {
                        todos.value.push({
                            id: Date.now(),
                            text: newTodo.value.trim(),
                            completed: false
                        })
                        newTodo.value = ''
                    }
                }
                
                const toggleTodo = (id) => {
                    const todo = todos.value.find(t => t.id === id)
                    if (todo) {
                        todo.completed = !todo.completed
                    }
                }
                
                const removeTodo = (id) => {
                    todos.value = todos.value.filter(t => t.id !== id)
                }
                
                // User methods
                const addRandomUser = () => {
                    const names = ['David Wilson', 'Emma Davis', 'Frank Miller', 'Grace Lee', 'Henry Taylor']
                    const roles = ['User', 'Moderator', 'Admin']
                    const randomName = names[Math.floor(Math.random() * names.length)]
                    const randomRole = roles[Math.floor(Math.random() * roles.length)]
                    
                    users.value.push({
                        id: Date.now(),
                        name: randomName,
                        email: randomName.toLowerCase().replace(' ', '.') + '@example.com',
                        role: randomRole,
                        isOnline: Math.random() > 0.5
                    })
                }
                
                const toggleUserStatus = (id) => {
                    const user = users.value.find(u => u.id === id)
                    if (user) {
                        user.isOnline = !user.isOnline
                    }
                }
                
                const removeUser = (id) => {
                    users.value = users.value.filter(u => u.id !== id)
                }
                
                const toggleAllOnline = () => {
                    const allOnline = users.value.every(user => user.isOnline)
                    users.value.forEach(user => {
                        user.isOnline = !allOnline
                    })
                }
                
                return {
                    // Basic conditional data
                    isLoggedIn,
                    userType,
                    showComparison,
                    
                    // Todo data and methods
                    newTodo,
                    todos,
                    totalTodos,
                    completedTodos,
                    remainingTodos,
                    addTodo,
                    toggleTodo,
                    removeTodo,
                    
                    // User data and methods
                    users,
                    filterStatus,
                    filteredUsers,
                    addRandomUser,
                    toggleUserStatus,
                    removeUser,
                    toggleAllOnline,
                    
                    // Department data
                    departments
                }
            }
        }).mount('#app')
    </script>
</body>
</html>
```

## Understanding Conditional Rendering

### 1. Basic `v-if` - Show or Hide Elements

```html
<!-- Only shows if awesome is true -->
<h1 v-if="awesome">Vue is awesome!</h1>

<!-- Shows the opposite -->
<h1 v-else>Oh no 😢</h1>
```

The element is completely **removed** from the DOM when the condition is false!

### 2. Multiple Conditions with `v-else-if`

```html
<div v-if="score >= 90">A+ Excellent!</div>
<div v-else-if="score >= 80">B+ Good job!</div>
<div v-else-if="score >= 70">C+ Not bad!</div>
<div v-else>Need more practice!</div>
```

Perfect for handling multiple scenarios!

### 3. `v-show` - CSS-Based Hiding

```html
<div v-show="isVisible">I'm hidden with CSS, not removed!</div>
```

The element stays in the DOM but gets `display: none` when hidden.

### 4. `v-if` vs `v-show` - When to Use Which?

| Use `v-if` when: | Use `v-show` when: |
|------------------|-------------------|
| ✅ Condition rarely changes | ✅ Toggling frequently |
| ✅ Want lazy loading | ✅ Element is expensive to create |
| ✅ Condition affects many elements | ✅ Simple show/hide |
| ✅ Want to save memory | ✅ Need instant toggling |

### 5. Template Groups with `v-if`

```html
<template v-if="loading">
  <h2>Loading...</h2>
  <p>Please wait...</p>
  <div class="spinner"></div>
</template>
```

Use `<template>` to group multiple elements without adding extra HTML!

## Understanding List Rendering with `v-for`

### 1. Basic List Rendering

The `v-for` directive uses the special syntax `item in items`:

```html
<!-- Array of strings -->
<li v-for="item in items" :key="item">{{ item }}</li>

<!-- Array of objects -->
<div v-for="user in users" :key="user.id">
  <h3>{{ user.name }}</h3>
  <p>{{ user.email }}</p>
</div>
```

Think of it like JavaScript's `forEach` - Vue goes through each item and creates HTML for it!

### 2. Getting the Index Too

```html
<li v-for="(item, index) in items" :key="item.id">
  {{ index + 1 }}. {{ item.name }}
</li>
```

Just like `array.forEach((item, index) => {})` in JavaScript!

### 3. Object Destructuring in v-for

You can destructure objects directly in the `v-for`:

```html
<!-- Instead of item.message -->
<li v-for="{ message, id } in items" :key="id">
  {{ message }}
</li>

<!-- With index too -->
<li v-for="({ message, id }, index) in items" :key="id">
  {{ index }}: {{ message }}
</li>
```

### 4. Looping Through Object Properties

```html
<!-- Just values -->
<li v-for="value in userProfile" :key="value">{{ value }}</li>

<!-- Keys and values -->
<li v-for="(value, key) in userProfile" :key="key">
  <strong>{{ key }}:</strong> {{ value }}
</li>

<!-- Values, keys, and index -->
<li v-for="(value, key, index) in userProfile" :key="key">
  {{ index }}. {{ key }}: {{ value }}
</li>
```

### 5. Number Ranges

```html
<!-- Creates numbers 1, 2, 3, 4, 5 -->
<span v-for="n in 5" :key="n">{{ n }}</span>
```

Great for pagination or numbered lists!

### 6. Template Groups with `v-for`

When you need multiple elements for each item:

```html
<template v-for="item in items" :key="item.id">
  <h3>{{ item.title }}</h3>
  <p>{{ item.description }}</p>
  <hr>
</template>
```

The `<template>` doesn't create an extra wrapper element!

### 7. Alternative Syntax: `of` instead of `in`

```html
<!-- These are exactly the same -->
<div v-for="item in items" :key="item.id">
<div v-for="item of items" :key="item.id">
```

Use whichever feels more natural to you!

## Working with Dynamic Lists - Array Methods

### 1. Mutating Methods (Change the Original Array)

Vue automatically detects when you use these methods and updates the display:

```javascript
// These methods change the original array and Vue will update the UI
todos.value.push(newTodo)           // Add to end
todos.value.pop()                   // Remove from end  
todos.value.unshift(newTodo)        // Add to beginning
todos.value.shift()                 // Remove from beginning
todos.value.splice(2, 1, newTodo)   // Remove/add at specific position
todos.value.sort()                  // Sort the array
todos.value.reverse()               // Reverse the order
```

### 2. Non-Mutating Methods (Create New Arrays)

```javascript
// These create new arrays - assign the result back to your ref
todos.value = todos.value.filter(todo => !todo.completed)
todos.value = todos.value.concat(newTodos)
todos.value = todos.value.slice(0, 5) // First 5 items
```

**Vue is smart!** Even when you replace the entire array, Vue reuses DOM elements efficiently.

### 3. Filtered and Sorted Lists with Computed Properties

Instead of changing your original data, create computed properties:

```javascript
// Original data stays unchanged
const todos = ref([
  { id: 1, text: 'Learn Vue', completed: false },
  { id: 2, text: 'Build app', completed: true }
])

// Computed properties for different views
const completedTodos = computed(() => {
  return todos.value.filter(todo => todo.completed)
})

const activeTodos = computed(() => {
  return todos.value.filter(todo => !todo.completed)
})

const sortedTodos = computed(() => {
  // ⚠️ Important: Create a copy before sorting!
  return [...todos.value].sort((a, b) => a.text.localeCompare(b.text))
})
```

```html
<div v-for="todo in activeTodos" :key="todo.id">{{ todo.text }}</div>
```

### 4. Using Methods for Filtering (When Computed Properties Aren't Enough)

```javascript
const getFilteredUsers = (role) => {
  return users.value.filter(user => user.role === role)
}
```

```html
<div v-for="admin in getFilteredUsers('admin')" :key="admin.id">
  {{ admin.name }}
</div>
```

**Important:** Be careful with `sort()` and `reverse()` in computed properties!

```javascript
// ❌ Wrong: Mutates original array
const sortedItems = computed(() => {
  return items.value.sort() // This changes the original!
})

// ✅ Correct: Create a copy first
const sortedItems = computed(() => {
  return [...items.value].sort() // Safe!
})
```

## Common Beginner Questions

### Q: What happens if I don't use `:key` in my lists?
**A:** Vue will show a warning, and you might get weird behavior when your list changes (like form inputs showing wrong values).

### Q: Can I use the array index as a key?
**A:** Only if your list never changes order and items are never added/removed from the middle. Generally, use a unique ID instead.

### Q: When should I use `v-if` vs `v-show`?
**A:** 
- **`v-if`**: When the condition rarely changes or when you want to save memory
- **`v-show`**: When you're toggling frequently (like tabs or dropdowns)

### Q: Can I use `v-for` and `v-if` on the same element?
**A:** It's not recommended! `v-if` has higher priority than `v-for`, which can cause issues. Use one of these patterns instead:

```html
<!-- ❌ Not recommended -->
<div v-for="user in users" v-if="user.isActive" :key="user.id">

<!-- ✅ Option 1: Template wrapper -->
<template v-for="user in users" :key="user.id">
  <div v-if="user.isActive">{{ user.name }}</div>
</template>

<!-- ✅ Option 2: Computed property (best) -->
<div v-for="user in activeUsers" :key="user.id">{{ user.name }}</div>

<!-- ✅ Option 3: Container-level v-if -->
<div v-if="shouldShowUsers">
  <div v-for="user in users" :key="user.id">{{ user.name }}</div>
</div>
```

### Q: Can I use object destructuring in `v-for`?
**A:** Yes! It's really handy:

```html
<!-- Instead of item.name, item.email -->
<div v-for="{ name, email, id } in users" :key="id">
  <h3>{{ name }}</h3>
  <p>{{ email }}</p>
</div>

<!-- With index too -->
<div v-for="({ name, id }, index) in users" :key="id">
  {{ index + 1 }}. {{ name }}
</div>
```

### Q: How do I handle very large lists efficiently?
**A:** 
- Use computed properties for filtering/sorting instead of methods
- Consider virtual scrolling for thousands of items
- Use `v-show` instead of `v-if` for frequent toggles within lists
- Keep your `:key` values simple (avoid complex calculations)

### Q: Can I loop through numbers?
**A:** Absolutely! Great for pagination or numbered lists:

```html
<!-- Creates: 1, 2, 3, 4, 5 -->
<span v-for="n in 5" :key="n">{{ n }}</span>

<!-- Page numbers -->
<button v-for="page in totalPages" :key="page" @click="goToPage(page)">
  {{ page }}
</button>
```

## Fun Challenges to Try! 🎯

### Challenge 1: Build a Shopping Cart
Create a list of products with add/remove functionality and conditional "empty cart" message.

### Challenge 2: Create a User Management System
Build a user list with different roles, online status, and filtering options.

### Challenge 3: Make a Simple Blog
Display posts with conditional editing buttons based on user permissions.

### Challenge 4: Build a Task Manager
Create different task categories with conditional styling and filtering.

### Challenge 5: Design a Restaurant Menu
Show different menu sections with conditional "coming soon" items.

## What You've Mastered! 🏆

- ✅ **Conditional rendering** with `v-if`, `v-else-if`, and `v-else`
- ✅ **CSS-based toggling** with `v-show` 
- ✅ **When to use `v-if` vs `v-show`** for optimal performance
- ✅ **List rendering** with `v-for` and proper `:key` usage
- ✅ **Complex data structures** with nested objects and arrays
- ✅ **Dynamic styling and classes** based on data
- ✅ **Best practices** for maintainable conditional and list code

## What's Next?

In the next project, you'll combine everything you've learned to build a **complete calculator application**! You'll use conditional rendering, event handling, and dynamic content to create a fully functional calculator. Get ready to build something impressive! 🚀

## Pro Tips to Remember

- Always use unique `:key` attributes in `v-for`
- Choose `v-if` vs `v-show` based on usage patterns
- Use computed properties for filtered/sorted lists
- Group related elements with `<template>`
- Keep your conditions simple and readable
- Use descriptive variable names in `v-for`

You're building some serious Vue skills! These patterns will be the foundation of every dynamic app you create. Keep experimenting and having fun! 🌟