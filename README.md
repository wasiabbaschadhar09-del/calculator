# calculatorapple-calculator/
│
├── public/
│   └── favicon.ico
│
├── src/
│   ├── assets/
│   │   └── (optional images, icons)
│   │
│   ├── components/
│   │   └── Calculator.jsx
│       ───────────────────────────────
│       import React, { useState } from 'react'
│       import { motion } from 'framer-motion'
│
│       export default function Calculator() {
│         const [display, setDisplay] = useState('0')
│         const [prevValue, setPrevValue] = useState(null)
│         const [operator, setOperator] = useState(null)
│         const [waitingForNewValue, setWaitingForNewValue] = useState(false)
│
│         const clear = () => {
│           setDisplay('0')
│           setPrevValue(null)
│           setOperator(null)
│           setWaitingForNewValue(false)
│         }
│
│         const inputDigit = (digit) => {
│           if (waitingForNewValue) {
│             setDisplay(String(digit))
│             setWaitingForNewValue(false)
│           } else {
│             setDisplay((prev) => (prev === '0' ? String(digit) : prev + digit))
│           }
│         }
│
│         const inputDot = () => {
│           if (waitingForNewValue) {
│             setDisplay('0.')
│             setWaitingForNewValue(false)
│             return
│           }
│           if (!display.includes('.')) {
│             setDisplay(display + '.')
│           }
│         }
│
│         const toggleSign = () => {
│           setDisplay((prev) => (prev.charAt(0) === '-' ? prev.slice(1) : '-' + prev))
│         }
│
│         const inputPercent = () => {
│           const value = parseFloat(display)
│           if (Number.isNaN(value)) return
│           setDisplay(String(value / 100))
│         }
│
│         const performOperation = (nextOperator) => {
│           const inputValue = parseFloat(display)
│
│           if (prevValue == null) {
│             setPrevValue(inputValue)
│           } else if (operator) {
│             const currentValue = prevValue || 0
│             const result = calculate(currentValue, inputValue, operator)
│             setPrevValue(result)
│             setDisplay(String(result))
│           }
│
│           setWaitingForNewValue(true)
│           setOperator(nextOperator)
│         }
│
│         const calculate = (left, right, op) => {
│           switch (op) {
│             case '+': return left + right
│             case '-': return left - right
│             case '×': return left * right
│             case '÷': return right === 0 ? 'Error' : left / right
│             default: return right
│           }
│         }
│
│         const handleEqual = () => {
│           const inputValue = parseFloat(display)
│           if (operator && prevValue != null) {
│             const result = calculate(prevValue, inputValue, operator)
│             setDisplay(String(result))
│             setPrevValue(null)
│             setOperator(null)
│             setWaitingForNewValue(true)
│           }
│         }
│
│         const Btn = ({ children, onClick, type = 'number', wide = false }) => {
│           const base = 'flex items-center justify-center select-none font-medium'
│           const size = wide ? 'col-span-2' : ''
│           let style = ''
│           if (type === 'function') style = 'bg-gray-300 dark:bg-gray-700 text-black'
│           if (type === 'operator') style = 'bg-orange-500 text-white'
│           if (type === 'number') style = 'bg-gray-100 dark:bg-gray-800 text-black'
│
│           return (
│             <motion.button whileTap={{ scale: 0.95 }} onClick={onClick}
│               className={`${base} ${size} ${style} rounded-2xl p-4 sm:p-5 shadow-inner sm:shadow-none`}>
│               <span className={`text-xl sm:text-2xl`}>{children}</span>
│             </motion.button>
│           )
│         }
│
│         return (
│           <div className=\"min-h-screen flex items-center justify-center bg-gradient-to-b from-gray-200 to-white dark:from-gray-900 dark:to-gray-800 p-6\">
│             <div className=\"w-full max-w-sm sm:max-w-md\">
│               <div className=\"bg-black/70 rounded-3xl p-4 sm:p-6 shadow-2xl\">
│                 <div className=\"w-full bg-transparent rounded-xl p-6 text-right\">
│                   <div className=\"text-gray-400 text-sm sm:text-base\">Calculator</div>
│                   <div className=\"mt-2 break-words text-white text-4xl sm:text-5xl font-light\" style={{ wordBreak: 'break-all' }}>{display}</div>
│                 </div>
│                 <div className=\"mt-4 grid grid-cols-4 gap-3\">
│                   <Btn type=\"function\" onClick={clear}>AC</Btn>
│                   <Btn type=\"function\" onClick={toggleSign}>+/-</Btn>
│                   <Btn type=\"function\" onClick={inputPercent}>%</Btn>
│                   <Btn type=\"operator\" onClick={() => performOperation('÷')}>÷</Btn>
│                   <Btn onClick={() => inputDigit(7)}>7</Btn>
│                   <Btn onClick={() => inputDigit(8)}>8</Btn>
│                   <Btn onClick={() => inputDigit(9)}>9</Btn>
│                   <Btn type=\"operator\" onClick={() => performOperation('×')}>×</Btn>
│                   <Btn onClick={() => inputDigit(4)}>4</Btn>
│                   <Btn onClick={() => inputDigit(5)}>5</Btn>
│                   <Btn onClick={() => inputDigit(6)}>6</Btn>
│                   <Btn type=\"operator\" onClick={() => performOperation('-')}>-</Btn>
│                   <Btn onClick={() => inputDigit(1)}>1</Btn>
│                   <Btn onClick={() => inputDigit(2)}>2</Btn>
│                   <Btn onClick={() => inputDigit(3)}>3</Btn>
│                   <Btn type=\"operator\" onClick={() => performOperation('+')}>+</Btn>
│                   <Btn wide onClick={() => inputDigit(0)}>0</Btn>
│                   <Btn onClick={inputDot}>.</Btn>
│                   <Btn type=\"operator\" onClick={handleEqual}>=</Btn>
│                 </div>
│               </div>
│               <div className=\"mt-3 text-center text-xs text-gray-500\">Responsive & mobile-friendly — Apple-style layout</div>
│             </div>
│           </div>
│         )
│       }
│
│   ├── App.jsx
│       import Calculator from './components/Calculator'
│       export default function App() {
│         return <Calculator />
│       }
│
│   ├── main.jsx
│       import React from 'react'
│       import ReactDOM from 'react-dom/client'
│       import App from './App'
│       import './index.css'
│       ReactDOM.createRoot(document.getElementById('root')).render(<App />)
│
│   ├── index.css
│       @tailwind base;
│       @tailwind components;
│       @tailwind utilities;
│       body {@apply bg-gray-100 text-gray-900;}
│
├── .gitignore
│   node_modules
│   dist
│   .DS_Store
│
├── index.html
│   <!doctype html>
│   <html lang=\"en\">
│     <head>
│       <meta charset=\"UTF-8\" />
│       <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\" />
│       <title>Apple Calculator</title>
│     </head>
│     <body>
│       <div id=\"root\"></div>
│       <script type=\"module\" src=\"/src/main.jsx\"></script>
│     </body>
│   </html>
│
├── package.json
│   {
│     \"name\": \"apple-calculator\",
│     \"version\": \"1.0.0\",
│     \"scripts\": {
│       \"dev\": \"vite\",
│       \"build\": \"vite build\",
│       \"preview\": \"vite preview\"
│     },
│     \"dependencies\": {
│       \"react\": \"^18.0.0\",
│       \"react-dom\": \"^18.0.0\",
│       \"framer-motion\": \"^11.0.0\"
│     },
│     \"devDependencies\": {
│       \"vite\": \"^5.0.0\",
│       \"tailwindcss\": \"^3.0.0\",
│       \"autoprefixer\": \"^10.0.0\",
│       \"postcss\": \"^8.0.0\"
│     }
│   }
│
├── postcss.config.js
│   export default { plugins: { tailwindcss: {}, autoprefixer: {} } }
│
├── tailwind.config.js
│   export default { content: [\"./index.html\", \"./src/**/*.{js,jsx}\"], theme: { extend: {} }, plugins: [] }
│
├── vite.config.js
│   import { defineConfig } from 'vite'
│   import react from '@vitejs/plugin-react'
│   export default defineConfig({ plugins: [react()] })
│
└── README.md
    # Apple-like Calculator
    A responsive React + Tailwind CSS calculator inspired by Apple's design.

    ## Run locally
    npm install
    npm run dev

    ## Build for production
    npm run build
