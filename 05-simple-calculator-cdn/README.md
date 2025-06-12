# 05 - Building Your First Real App: Calculator! 🧮

## Welcome to Your Big Project!
This is it - your first complete Vue application! You're going to build a fully functional calculator that combines EVERYTHING you've learned: reactive data, event handling, conditional rendering, and dynamic content. By the end, you'll have built something you can proudly show off!

## What Makes This Special
This isn't just another tutorial - this is your first **real application** that combines all Vue fundamentals:
- **Reactive data** for calculator state and display
- **Event handling** for button clicks and keyboard input  
- **Conditional rendering** for different calculator states
- **Methods** for all calculator operations
- **Dynamic content** that updates as you calculate

## Your Calculator Features
✅ Basic arithmetic (+, -, ×, ÷)  
✅ Clear and Clear Entry functions  
✅ Decimal point handling  
✅ Error handling (like division by zero)  
✅ Keyboard support  
✅ Memory functions (bonus!)  
✅ Calculation history  
✅ Responsive design  

## The Complete Calculator App

```html
<!DOCTYPE html>
<html>
<head>
    <title>Vue Calculator - My First Real App!</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .calculator-container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            overflow: hidden;
            width: 100%;
            max-width: 400px;
        }
        
        .calculator-header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        .calculator-header h1 {
            margin: 0;
            font-size: 1.5em;
        }
        
        .calculator-header p {
            margin: 5px 0 0 0;
            opacity: 0.9;
            font-size: 0.9em;
        }
        
        .display-section {
            padding: 20px;
            background: #f8f9fa;
        }
        
        .main-display {
            background: #000;
            color: #00ff41;
            font-family: 'Courier New', monospace;
            font-size: 2.5em;
            padding: 15px;
            border-radius: 10px;
            text-align: right;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            border: 2px solid #333;
            overflow: hidden;
            word-break: break-all;
        }
        
        .main-display.error {
            color: #ff4444;
            animation: shake 0.5s;
        }
        
        .secondary-display {
            color: #666;
            font-size: 1em;
            text-align: right;
            margin-bottom: 10px;
            min-height: 20px;
            font-family: 'Courier New', monospace;
        }
        
        .status-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 10px 0;
            font-size: 0.9em;
            color: #666;
        }
        
        .memory-indicator {
            background: #007bff;
            color: white;
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 0.8em;
        }
        
        .buttons-section {
            padding: 20px;
        }
        
        .button-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }
        
        .btn {
            border: none;
            border-radius: 10px;
            font-size: 1.2em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }
        
        .btn:active {
            transform: translateY(0);
        }
        
        .btn-number {
            background: #e9ecef;
            color: #333;
        }
        
        .btn-number:hover {
            background: #dee2e6;
        }
        
        .btn-operator {
            background: #007bff;
            color: white;
        }
        
        .btn-operator:hover {
            background: #0056b3;
        }
        
        .btn-equals {
            background: #28a745;
            color: white;
            grid-column: span 2;
        }
        
        .btn-equals:hover {
            background: #1e7e34;
        }
        
        .btn-clear {
            background: #dc3545;
            color: white;
        }
        
        .btn-clear:hover {
            background: #c82333;
        }
        
        .btn-function {
            background: #6c757d;
            color: white;
        }
        
        .btn-function:hover {
            background: #545b62;
        }
        
        .btn-zero {
            grid-column: span 2;
        }
        
        .history-section {
            border-top: 1px solid #eee;
            padding: 20px;
            max-height: 200px;
            overflow-y: auto;
        }
        
        .history-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .history-item {
            padding: 8px 12px;
            background: #f8f9fa;
            margin: 5px 0;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.2s;
            font-family: 'Courier New', monospace;
            font-size: 0.9em;
        }
        
        .history-item:hover {
            background: #e9ecef;
        }
        
        .keyboard-help {
            background: #e7f3ff;
            padding: 15px;
            margin-top: 20px;
            border-radius: 10px;
            font-size: 0.85em;
            color: #0c5460;
        }
        
        .keyboard-help h4 {
            margin-bottom: 8px;
            color: #0c5460;
        }
        
        .keyboard-shortcuts {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 5px;
            margin-top: 10px;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }
        
        @media (max-width: 480px) {
            .calculator-container {
                margin: 10px;
            }
            
            .main-display {
                font-size: 2em;
            }
            
            .btn {
                min-height: 50px;
                font-size: 1.1em;
            }
        }
    </style>
</head>
<body>
    <!-- Include Vue -->
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

    <div id="app">
        <div class="calculator-container" @keydown="handleKeyboard" tabindex="0">
            <!-- Header -->
            <div class="calculator-header">
                <h1>🧮 Vue Calculator</h1>
                <p>My First Real Vue App!</p>
            </div>

            <!-- Display Section -->
            <div class="display-section">
                <div class="secondary-display">
                    {{ calculation }}
                </div>
                
                <div class="main-display" :class="{ error: hasError }">
                    {{ currentDisplay }}
                </div>
                
                <div class="status-bar">
                    <div>
                        <span v-if="memory !== 0" class="memory-indicator">M: {{ memory }}</span>
                    </div>
                    <div>
                        <small>{{ lastOperation }}</small>
                    </div>
                </div>
            </div>

            <!-- Button Grid -->
            <div class="buttons-section">
                <div class="button-grid">
                    <!-- First Row -->
                    <button @click="clearAll" class="btn btn-clear">AC</button>
                    <button @click="clearEntry" class="btn btn-function">CE</button>
                    <button @click="memoryRecall" class="btn btn-function" :disabled="memory === 0">MR</button>
                    <button @click="setOperation('÷')" class="btn btn-operator">÷</button>
                    
                    <!-- Second Row -->
                    <button @click="inputNumber(7)" class="btn btn-number">7</button>
                    <button @click="inputNumber(8)" class="btn btn-number">8</button>
                    <button @click="inputNumber(9)" class="btn btn-number">9</button>
                    <button @click="setOperation('×')" class="btn btn-operator">×</button>
                    
                    <!-- Third Row -->
                    <button @click="inputNumber(4)" class="btn btn-number">4</button>
                    <button @click="inputNumber(5)" class="btn btn-number">5</button>
                    <button @click="inputNumber(6)" class="btn btn-number">6</button>
                    <button @click="setOperation('-')" class="btn btn-operator">-</button>
                    
                    <!-- Fourth Row -->
                    <button @click="inputNumber(1)" class="btn btn-number">1</button>
                    <button @click="inputNumber(2)" class="btn btn-number">2</button>
                    <button @click="inputNumber(3)" class="btn btn-number">3</button>
                    <button @click="setOperation('+')" class="btn btn-operator">+</button>
                    
                    <!-- Fifth Row -->
                    <button @click="inputNumber(0)" class="btn btn-number btn-zero">0</button>
                    <button @click="inputDecimal" class="btn btn-number">.</button>
                    <button @click="calculate" class="btn btn-equals">=</button>
                    
                    <!-- Memory Row -->
                    <button @click="memoryAdd" class="btn btn-function">M+</button>
                    <button @click="memorySubtract" class="btn btn-function">M-</button>
                    <button @click="memoryClear" class="btn btn-function">MC</button>
                    <button @click="percentage" class="btn btn-function">%</button>
                </div>
            </div>

            <!-- History Section -->
            <div v-if="history.length > 0" class="history-section">
                <div class="history-header">
                    <h4>📜 Calculation History</h4>
                    <button @click="clearHistory" class="btn btn-function" style="padding: 5px 10px; font-size: 0.8em;">Clear</button>
                </div>
                
                <div v-if="history.length === 0" style="text-align: center; color: #666; font-style: italic;">
                    No calculations yet
                </div>
                
                <div v-for="(item, index) in history.slice().reverse()" :key="index" 
                     @click="useHistoryResult(item.result)" class="history-item">
                    {{ item.expression }} = {{ item.result }}
                </div>
            </div>

            <!-- Keyboard Help -->
            <div class="keyboard-help">
                <h4>⌨️ Keyboard Shortcuts</h4>
                <div class="keyboard-shortcuts">
                    <div><strong>Numbers:</strong> 0-9</div>
                    <div><strong>Operations:</strong> + - * /</div>
                    <div><strong>Calculate:</strong> Enter or =</div>
                    <div><strong>Clear:</strong> Escape or C</div>
                    <div><strong>Decimal:</strong> . (period)</div>
                    <div><strong>Backspace:</strong> Delete last</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const { createApp, ref, computed } = Vue

        createApp({
            setup() {
                // Core calculator state
                const currentInput = ref('0')
                const previousInput = ref(null)
                const operator = ref(null)
                const waitingForNewInput = ref(false)
                const hasError = ref(false)
                const lastOperation = ref('')
                
                // Memory and history
                const memory = ref(0)
                const history = ref([])
                
                // Computed properties
                const currentDisplay = computed(() => {
                    if (hasError.value) return 'Error'
                    return formatDisplay(currentInput.value)
                })
                
                const calculation = computed(() => {
                    if (hasError.value) return 'Please clear and try again'
                    if (operator.value && previousInput.value !== null && !waitingForNewInput.value) {
                        return `${formatDisplay(previousInput.value)} ${operator.value} ${formatDisplay(currentInput.value)}`
                    }
                    if (operator.value && previousInput.value !== null) {
                        return `${formatDisplay(previousInput.value)} ${operator.value}`
                    }
                    return ''
                })
                
                // Helper functions
                const formatDisplay = (num) => {
                    if (num === null || num === undefined) return '0'
                    const numStr = String(num)
                    
                    // Handle very long numbers
                    if (numStr.length > 12) {
                        const numFloat = parseFloat(num)
                        if (Math.abs(numFloat) > 999999999999) {
                            return numFloat.toExponential(5)
                        }
                        return numFloat.toPrecision(10).replace(/\.?0+$/, '')
                    }
                    
                    return numStr
                }
                
                const addToHistory = (expression, result) => {
                    history.value.push({
                        expression,
                        result: formatDisplay(result),
                        timestamp: new Date().toLocaleTimeString()
                    })
                    
                    // Keep only last 10 calculations
                    if (history.value.length > 10) {
                        history.value.shift()
                    }
                }
                
                // Number input
                const inputNumber = (num) => {
                    if (hasError.value) {
                        clearAll()
                    }
                    
                    if (waitingForNewInput.value) {
                        currentInput.value = String(num)
                        waitingForNewInput.value = false
                    } else {
                        if (currentInput.value === '0') {
                            currentInput.value = String(num)
                        } else {
                            currentInput.value += String(num)
                        }
                    }
                    
                    lastOperation.value = `Entered ${num}`
                }
                
                // Decimal input
                const inputDecimal = () => {
                    if (hasError.value) {
                        clearAll()
                    }
                    
                    if (waitingForNewInput.value) {
                        currentInput.value = '0.'
                        waitingForNewInput.value = false
                    } else if (!currentInput.value.includes('.')) {
                        currentInput.value += '.'
                    }
                    
                    lastOperation.value = 'Added decimal point'
                }
                
                // Operations
                const setOperation = (op) => {
                    if (hasError.value) return
                    
                    // If we already have an operation and aren't waiting for new input, calculate first
                    if (operator.value && !waitingForNewInput.value) {
                        calculate()
                        if (hasError.value) return
                    }
                    
                    previousInput.value = currentInput.value
                    operator.value = op
                    waitingForNewInput.value = true
                    lastOperation.value = `Selected ${op === '×' ? 'multiply' : op === '÷' ? 'divide' : op === '+' ? 'add' : 'subtract'}`
                }
                
                // Calculate result
                const calculate = () => {
                    if (!operator.value || previousInput.value === null || hasError.value) return
                    
                    const prev = parseFloat(previousInput.value)
                    const current = parseFloat(currentInput.value)
                    let result
                    
                    const expression = `${formatDisplay(previousInput.value)} ${operator.value} ${formatDisplay(currentInput.value)}`
                    
                    try {
                        switch (operator.value) {
                            case '+':
                                result = prev + current
                                break
                            case '-':
                                result = prev - current
                                break
                            case '×':
                                result = prev * current
                                break
                            case '÷':
                                if (current === 0) {
                                    throw new Error('Division by zero')
                                }
                                result = prev / current
                                break
                            default:
                                return
                        }
                        
                        // Check for invalid results
                        if (!isFinite(result)) {
                            throw new Error('Invalid calculation')
                        }
                        
                        addToHistory(expression, result)
                        currentInput.value = String(result)
                        operator.value = null
                        previousInput.value = null
                        waitingForNewInput.value = true
                        lastOperation.value = `Calculated: ${expression} = ${formatDisplay(result)}`
                        
                    } catch (error) {
                        hasError.value = true
                        currentInput.value = 'Error'
                        lastOperation.value = error.message
                    }
                }
                
                // Clear functions
                const clearAll = () => {
                    currentInput.value = '0'
                    previousInput.value = null
                    operator.value = null
                    waitingForNewInput.value = false
                    hasError.value = false
                    lastOperation.value = 'Cleared all'
                }
                
                const clearEntry = () => {
                    currentInput.value = '0'
                    waitingForNewInput.value = false
                    hasError.value = false
                    lastOperation.value = 'Cleared entry'
                }
                
                // Memory functions
                const memoryAdd = () => {
                    memory.value += parseFloat(currentInput.value) || 0
                    lastOperation.value = `Added ${currentInput.value} to memory`
                }
                
                const memorySubtract = () => {
                    memory.value -= parseFloat(currentInput.value) || 0
                    lastOperation.value = `Subtracted ${currentInput.value} from memory`
                }
                
                const memoryRecall = () => {
                    if (memory.value !== 0) {
                        currentInput.value = String(memory.value)
                        waitingForNewInput.value = true
                        lastOperation.value = `Recalled ${memory.value} from memory`
                    }
                }
                
                const memoryClear = () => {
                    memory.value = 0
                    lastOperation.value = 'Memory cleared'
                }
                
                // Percentage
                const percentage = () => {
                    const current = parseFloat(currentInput.value)
                    currentInput.value = String(current / 100)
                    lastOperation.value = `Converted ${current} to ${current / 100}%`
                }
                
                // History functions
                const clearHistory = () => {
                    history.value = []
                    lastOperation.value = 'History cleared'
                }
                
                const useHistoryResult = (result) => {
                    currentInput.value = result
                    waitingForNewInput.value = true
                    lastOperation.value = `Used result: ${result}`
                }
                
                // Keyboard support
                const handleKeyboard = (event) => {
                    const key = event.key
                    
                    // Prevent default for calculator keys
                    if ('0123456789+-*/.=Enter'.includes(key) || key === 'Escape' || key === 'Backspace') {
                        event.preventDefault()
                    }
                    
                    if (key >= '0' && key <= '9') {
                        inputNumber(parseInt(key))
                    } else if (key === '.') {
                        inputDecimal()
                    } else if (key === '+') {
                        setOperation('+')
                    } else if (key === '-') {
                        setOperation('-')
                    } else if (key === '*') {
                        setOperation('×')
                    } else if (key === '/') {
                        setOperation('÷')
                    } else if (key === '=' || key === 'Enter') {
                        calculate()
                    } else if (key === 'Escape' || key.toLowerCase() === 'c') {
                        clearAll()
                    } else if (key === 'Backspace') {
                        if (currentInput.value.length > 1 && currentInput.value !== '0') {
                            currentInput.value = currentInput.value.slice(0, -1)
                        } else {
                            currentInput.value = '0'
                        }
                        lastOperation.value = 'Deleted last digit'
                    }
                }
                
                return {
                    // State
                    currentDisplay,
                    calculation,
                    hasError,
                    lastOperation,
                    memory,
                    history,
                    
                    // Methods
                    inputNumber,
                    inputDecimal,
                    setOperation,
                    calculate,
                    clearAll,
                    clearEntry,
                    memoryAdd,
                    memorySubtract,
                    memoryRecall,
                    memoryClear,
                    percentage,
                    clearHistory,
                    useHistoryResult,
                    handleKeyboard
                }
            }
        }).mount('#app')
    </script>
</body>
</html>
```

## Understanding the Calculator Architecture

### 1. State Management with Reactive Data

```javascript
// Core calculator state
const currentInput = ref('0')        // What user is typing
const previousInput = ref(null)      // Previous number for calculations
const operator = ref(null)           // Current operation (+, -, ×, ÷)
const waitingForNewInput = ref(false) // Should next digit start new number?
const hasError = ref(false)          // Is calculator in error state?

// Additional features
const memory = ref(0)                // Memory storage
const history = ref([])              // Calculation history
```

This shows perfect **reactive data** usage - when any of these change, the display updates automatically!

### 2. Computed Properties for Dynamic Display

```javascript
const currentDisplay = computed(() => {
    if (hasError.value) return 'Error'
    return formatDisplay(currentInput.value)
})

const calculation = computed(() => {
    if (hasError.value) return 'Please clear and try again'
    if (operator.value && previousInput.value !== null) {
        return `${formatDisplay(previousInput.value)} ${operator.value} ${formatDisplay(currentInput.value)}`
    }
    return ''
})
```

**Computed properties** automatically update the display based on calculator state!

### 3. Event Handling for User Interactions

```javascript
// Button clicks
const inputNumber = (num) => { /* Handle number input */ }
const setOperation = (op) => { /* Handle operation buttons */ }

// Keyboard support
const handleKeyboard = (event) => {
    if (event.key >= '0' && event.key <= '9') {
        inputNumber(parseInt(event.key))
    }
    // ... more keyboard handling
}
```

```html
<!-- Button events -->
<button @click="inputNumber(7)">7</button>
<button @click="setOperation('+')">+</button>

<!-- Keyboard events -->
<div @keydown="handleKeyboard" tabindex="0">
```

Perfect **event handling** - both clicks and keyboard work seamlessly!

### 4. Conditional Rendering for Different States

```html
<!-- Error state styling -->
<div class="main-display" :class="{ error: hasError }">
    {{ currentDisplay }}
</div>

<!-- Memory indicator only when memory has value -->
<span v-if="memory !== 0" class="memory-indicator">
    M: {{ memory }}
</span>

<!-- History section only when there are calculations -->
<div v-if="history.length > 0" class="history-section">
    <!-- History content -->
</div>

<!-- Disable memory recall when memory is empty -->
<button @click="memoryRecall" :disabled="memory === 0">MR</button>
```

**Conditional rendering** makes the UI smart and responsive!

### 5. List Rendering for History

```html
<div v-for="(item, index) in history.slice().reverse()" :key="index" 
     @click="useHistoryResult(item.result)" class="history-item">
    {{ item.expression }} = {{ item.result }}
</div>
```

**List rendering** shows calculation history dynamically!

## Key Programming Concepts You're Using

### 1. State Management
Your calculator maintains state across user interactions:
```javascript
// The calculator "remembers" where it is in a calculation
if (operator.value && !waitingForNewInput.value) {
    calculate() // Chain calculations: 2 + 3 + 4
}
```

### 2. Error Handling
```javascript
try {
    if (current === 0) {
        throw new Error('Division by zero')
    }
    result = prev / current
} catch (error) {
    hasError.value = true
    lastOperation.value = error.message
}
```

### 3. Data Formatting
```javascript
const formatDisplay = (num) => {
    // Handle very long numbers with scientific notation
    if (numStr.length > 12) {
        return numFloat.toExponential(5)
    }
    return numStr
}
```

### 4. User Experience Features
- **Keyboard support** for power users
- **Visual feedback** with hover effects and animations
- **Error prevention** (disable buttons when inappropriate)
- **History functionality** for reviewing calculations

## What Makes This a "Real App"

### 1. Production-Ready Features
✅ **Error handling** - Graceful failure recovery  
✅ **Input validation** - Prevents invalid operations  
✅ **Responsive design** - Works on mobile and desktop  
✅ **Keyboard accessibility** - Full keyboard support  
✅ **Visual feedback** - Clear user interface cues  

### 2. Advanced Functionality
✅ **Memory functions** - M+, M-, MR, MC  
✅ **Calculation history** - Review and reuse results  
✅ **Chained calculations** - 2 + 3 + 4 without pressing equals  
✅ **Percentage calculations** - Business calculator features  
✅ **Number formatting** - Handles very large/small numbers  

### 3. Professional Code Organization
✅ **Separation of concerns** - Display, logic, and styling separated  
✅ **Reusable functions** - Clean, modular code  
✅ **Computed properties** - Efficient reactive updates  
✅ **Event handling** - Multiple input methods supported  

## Challenges to Extend Your Calculator

### 🚀 Beginner Challenges
1. **Add square root function** - Add a √ button
2. **Add sign toggle** - +/- button to make numbers negative
3. **Add more history** - Show timestamps for calculations
4. **Custom themes** - Add dark/light mode toggle

### 🔥 Intermediate Challenges  
1. **Scientific calculator** - Add sin, cos, tan, log functions
2. **Unit converter** - Length, weight, temperature conversions
3. **Expression parser** - Allow typing "2+3*4" directly
4. **Save/load** - Store calculator state in localStorage

### 💫 Advanced Challenges
1. **Graphing calculator** - Plot mathematical functions
2. **Programming calculator** - Binary, hex, octal modes
3. **Financial calculator** - Loan payments, interest calculations
4. **Multi-calculator** - Multiple calculator instances

## What You've Accomplished! 🏆

**🎯 Vue.js Mastery:**
- ✅ **Reactive data** with refs and computed properties
- ✅ **Event handling** for clicks and keyboard input
- ✅ **Conditional rendering** for dynamic UI states
- ✅ **List rendering** for history functionality
- ✅ **Component lifecycle** and state management

**💻 Programming Skills:**
- ✅ **Error handling** and user input validation
- ✅ **State management** across user interactions
- ✅ **Data formatting** and display logic
- ✅ **Event-driven programming** patterns
- ✅ **User experience** design principles

**🚀 Real-World Application:**
- ✅ Built a **complete, functional application**
- ✅ Implemented **professional features** (keyboard support, history, memory)
- ✅ Created **responsive, accessible** user interface
- ✅ Used **modern development** practices and patterns

## What's Next?

You've just built your first real Vue application! 🎉 This calculator demonstrates that you understand:

- How Vue's reactivity system works
- How to handle complex user interactions  
- How to manage application state
- How to build professional user interfaces

In the next project, you'll learn about **Options API vs Composition API** - understanding different ways to organize your Vue code. This will prepare you for more advanced Vue development patterns!

## Pro Tips for Real Applications

### 1. Always Handle Edge Cases
```javascript
// Check for division by zero
if (current === 0) {
    throw new Error('Division by zero')
}

// Handle invalid numbers
if (!isFinite(result)) {
    throw new Error('Invalid calculation')
}
```

### 2. Provide Multiple Input Methods
- Mouse clicks for casual users
- Keyboard shortcuts for power users
- Touch-friendly design for mobile

### 3. Give Users Feedback
- Visual feedback for button presses
- Clear error messages
- Status indicators (memory, operations)

### 4. Keep State Simple
- Use reactive data for everything that changes
- Computed properties for derived values
- Methods for user actions

You should be incredibly proud of what you've built! This calculator is a real application that demonstrates professional Vue.js development skills. You're well on your way to becoming a Vue developer! 🌟