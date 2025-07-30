# Agent-AI-hackathpon
# My project for the hackathon with the help of AI agents
# 🎯 The problem statement is as Follows:
# As a student your task is to develop a AI-powered code debugger using AI agents.
# We know that Ai agents are powerful enough to create the code but the true talent lies here when we solve the issues that the agent Ai made debugger fails to recognize.Hence it's quite important to have physical involvement and inspection in the code creation for better results
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Code Debugger</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
            min-height: 100vh;
        }

        .header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            text-align: center;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.18);
        }

        .header h1 {
            color: #4a5568;
            font-size: 2.5rem;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .header p {
            color: #718096;
            font-size: 1.1rem;
        }

        .main-content {
            display: flex;
            gap: 20px;
            flex: 1;
        }

        .left-panel {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .right-panel {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .panel {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.18);
        }

        .panel h2 {
            color: #4a5568;
            margin-bottom: 15px;
            font-size: 1.4rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .code-input {
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .language-selector {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }

        .lang-btn {
            padding: 8px 16px;
            border: 2px solid #e2e8f0;
            background: white;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 500;
        }

        .lang-btn.active {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: #667eea;
        }

        .lang-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }

        textarea {
            flex: 1;
            min-height: 200px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            padding: 15px;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            resize: vertical;
            background: #f8fafc;
            transition: border-color 0.3s ease;
        }

        textarea:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .controls {
            display: flex;
            gap: 15px;
            margin-top: 15px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            font-size: 0.9rem;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }

        .btn-secondary {
            background: linear-gradient(135deg, #48bb78, #38a169);
            color: white;
        }

        .btn-danger {
            background: linear-gradient(135deg, #f56565, #e53e3e);
            color: white;
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
        }

        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .debug-output {
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .output-content {
            flex: 1;
            background: #1a202c;
            color: #e2e8f0;
            border-radius: 10px;
            padding: 20px;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            overflow-y: auto;
            min-height: 200px;
            white-space: pre-wrap;
            border: 2px solid #2d3748;
        }

        .loading {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            color: #667eea;
            font-weight: 500;
        }

        .spinner {
            width: 20px;
            height: 20px;
            border: 2px solid #e2e8f0;
            border-top: 2px solid #667eea;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .ai-insights {
            max-height: 300px;
            overflow-y: auto;
        }

        .insight-item {
            background: #f7fafc;
            border-left: 4px solid #667eea;
            padding: 15px;
            margin-bottom: 10px;
            border-radius: 5px;
        }

        .insight-item h4 {
            color: #4a5568;
            margin-bottom: 5px;
        }

        .insight-item p {
            color: #718096;
            font-size: 0.9rem;
        }

        .vector-store-status {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
            padding: 10px;
            background: #f0fff4;
            border: 1px solid #9ae6b4;
            border-radius: 8px;
            color: #276749;
            font-size: 0.9rem;
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #48bb78;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        .error {
            color: #e53e3e;
            background: #fed7d7;
            border: 1px solid #feb2b2;
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
        }

        .success {
            color: #276749;
            background: #f0fff4;
            border: 1px solid #9ae6b4;
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
        }

        @media (max-width: 768px) {
            .main-content {
                flex-direction: column;
            }
            
            .controls {
                justify-content: center;
            }
            
            .header h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🤖 AI Code Debugger</h1>
            <p>Advanced debugging with AI agents, vector stores, and intelligent analysis</p>
        </div>

        <div class="main-content">
            <div class="left-panel">
                <div class="panel code-input">
                    <h2>📝 Code Input</h2>
                    <div class="language-selector">
                        <button class="lang-btn active" data-lang="java">Java</button>
                        <button class="lang-btn" data-lang="python">Python</button>
                        <button class="lang-btn" data-lang="javascript">JavaScript</button>
                        <button class="lang-btn" data-lang="cpp">C++</button>
                    </div>
                    <textarea id="codeInput" placeholder="Paste your code here...">public class BuggyCalculator {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println("10 / 0 = " + calc.divide(10, 0));
        System.out.println("Array access: " + calc.getArrayElement(5));
    }
}

class Calculator {
    private int[] numbers = {1, 2, 3, 4, 5};
    
    public double divide(int a, int b) {
        return a / b; // Division by zero bug
    }
    
    public int getArrayElement(int index) {
        return numbers[index]; // Array index out of bounds bug
    }
}</textarea>
                    <div class="controls">
                        <button class="btn btn-primary" id="debugBtn">🔍 Debug Code</button>
                        <button class="btn btn-secondary" id="explainBtn">💡 Explain Issues</button>
                        <button class="btn btn-danger" id="clearBtn">🗑️ Clear</button>
                    </div>
                </div>
            </div>

            <div class="right-panel">
                <div class="panel debug-output">
                    <h2>🐛 Debug Results</h2>
                    <div class="vector-store-status">
                        <div class="status-dot"></div>
                        <span>Vector Store Connected - Knowledge Base Active</span>
                    </div>
                    <div class="output-content" id="debugOutput">
AI Code Debugger initialized successfully!

🔧 Features:
- Static code analysis
- AI-powered bug detection
- Vector store integration for pattern matching
- Real-time debugging suggestions
- Multi-language support

Ready to analyze your code...</div>
                </div>

                <div class="panel ai-insights">
                    <h2>🧠 AI Insights</h2>
                    <div id="aiInsights">
                        <div class="insight-item">
                            <h4>🎯 Pattern Recognition</h4>
                            <p>AI agent is ready to identify common bugs and anti-patterns using vector similarity search.</p>
                        </div>
                        <div class="insight-item">
                            <h4>📚 Knowledge Base</h4>
                            <p>Connected to debugging knowledge base with 10,000+ code patterns and solutions.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Simulated AI debugging system with vector store integration
        class AICodeDebugger {
            constructor() {
                this.vectorStore = new SimulatedVectorStore();
                this.knowledgeBase = new DebuggingKnowledgeBase();
                this.currentLanguage = 'java';
                this.analysisHistory = [];
                this.initializeEventListeners();
            }

            initializeEventListeners() {
                // Language selection
                document.querySelectorAll('.lang-btn').forEach(btn => {
                    btn.addEventListener('click', (e) => {
                        document.querySelectorAll('.lang-btn').forEach(b => b.classList.remove('active'));
                        e.target.classList.add('active');
                        this.currentLanguage = e.target.dataset.lang;
                        this.updateCodeTemplate();
                    });
                });

                // Debug button
                document.getElementById('debugBtn').addEventListener('click', () => {
                    this.debugCode();
                });

                // Explain button
                document.getElementById('explainBtn').addEventListener('click', () => {
                    this.explainIssues();
                });

                // Clear button
                document.getElementById('clearBtn').addEventListener('click', () => {
                    this.clearAll();
                });
            }

            updateCodeTemplate() {
                const templates = {
                    java: `public class BuggyCalculator {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println("10 / 0 = " + calc.divide(10, 0));
        System.out.println("Array access: " + calc.getArrayElement(5));
    }
}

class Calculator {
    private int[] numbers = {1, 2, 3, 4, 5};
    
    public double divide(int a, int b) {
        return a / b; // Division by zero bug
    }
    
    public int getArrayElement(int index) {
        return numbers[index]; // Array index out of bounds bug
    }
}`,
                    python: `def buggy_calculator():
    numbers = [1, 2, 3, 4, 5]
    
    # Division by zero bug
    result = 10 / 0
    print(f"10 / 0 = {result}")
    
    # Index out of bounds bug
    print(f"Array element: {numbers[10]}")
    
    # Undefined variable bug
    print(f"Undefined: {undefined_var}")

buggy_calculator()`,
                    javascript: `function buggyCalculator() {
    const numbers = [1, 2, 3, 4, 5];
    
    // Division by zero (returns Infinity in JS)
    console.log("10 / 0 =", 10 / 0);
    
    // Array access out of bounds (returns undefined)
    console.log("Array element:", numbers[10]);
    
    // Null pointer exception
    let obj = null;
    console.log("Object property:", obj.property);
}

buggyCalculator();`,
                    cpp: `#include <iostream>
#include <vector>

class Calculator {
private:
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    
public:
    double divide(int a, int b) {
        return a / b; // Division by zero bug
    }
    
    int getArrayElement(int index) {
        return numbers[index]; // No bounds checking
    }
};

int main() {
    Calculator calc;
    std::cout << "10 / 0 = " << calc.divide(10, 0) << std::endl;
    std::cout << "Array element: " << calc.getArrayElement(10) << std::endl;
    return 0;
}`
                };
                
                document.getElementById('codeInput').value = templates[this.currentLanguage];
            }

            async debugCode() {
                const code = document.getElementById('codeInput').value;
                const debugOutput = document.getElementById('debugOutput');
                const debugBtn = document.getElementById('debugBtn');
                
                if (!code.trim()) {
                    this.showError('Please enter some code to debug.');
                    return;
                }

                debugBtn.disabled = true;
                this.showLoading(debugOutput);

                try {
                    // Simulate AI processing delay
                    await this.delay(2000);
                    
                    // Perform static analysis
                    const issues = await this.performStaticAnalysis(code);
                    
                    // Query vector store for similar patterns
                    const similarPatterns = await this.vectorStore.findSimilarPatterns(code, this.currentLanguage);
                    
                    // Generate AI insights
                    const insights = await this.generateAIInsights(issues, similarPatterns);
                    this.displayDebugResults(issues, insights);
                    this.updateAIInsights(insights); 
                    this.showSuccess('Code analysis completed successfully!');
                } catch (error) {
                    this.showError('Error during debugging: ' + error.message);
                } finally {
                    debugBtn.disabled = false;
                }
            }

            async performStaticAnalysis(code) {
                const issues = [];
                const patterns = {
                    java: [
                        { pattern: /\/\s*0(?!\d)/, type: 'Runtime Error', severity: 'High', message: 'Division by zero detected' },
                        { pattern: /\[\s*\d+\s*\](?=(?:[^"]*"[^"]*")*[^"]*$)/, type: 'Array Access', severity: 'Medium', message: 'Potential array bounds issue' },
                        { pattern: /null\s*\./, type: 'NullPointer', severity: 'High', message: 'Null pointer dereference' },
                        { pattern: /catch\s*\(\s*Exception\s+\w+\s*\)\s*\{\s*\}/, type: 'Error Handling', severity: 'Low', message: 'Empty catch block' }
                    ],
                    python: [
                        { pattern: /\/\s*0(?!\d)/, type: 'Runtime Error', severity: 'High', message: 'Division by zero detected' },
                        { pattern: /\[\s*\d+\s*\]/, type: 'Array Access', severity: 'Medium', message: 'Potential list index out of range' },
                        { pattern: /except\s*:\s*pass/, type: 'Error Handling', severity: 'Low', message: 'Bare except clause' }
                    ],
                    javascript: [
                        { pattern: /null\s*\./, type: 'TypeError', severity: 'High', message: 'Cannot read property of null' },
                        { pattern: /undefined\s*\./, type: 'TypeError', severity: 'High', message: 'Cannot read property of undefined' },
                        { pattern: /==\s*null/, type: 'Comparison', severity: 'Low', message: 'Use strict equality (===) instead' }
                    ],
                    cpp: [
                        { pattern: /\/\s*0(?!\d)/, type: 'Runtime Error', severity: 'High', message: 'Division by zero detected' },
                        { pattern: /\[\s*\d+\s*\]/, type: 'Array Access', severity: 'High', message: 'Potential buffer overflow' },
                        { pattern: /delete\s+\w+\s*;.*delete\s+\w+\s*;/, type: 'Memory', severity: 'High', message: 'Double delete detected' }
                    ]
                };

                const langPatterns = patterns[this.currentLanguage] || patterns.java;
                
                langPatterns.forEach(({ pattern, type, severity, message }) => {
                    const matches = [...code.matchAll(new RegExp(pattern.source, 'g'))];
                    matches.forEach(match => {
                        const lineNumber = code.substring(0, match.index).split('\n').length;
                        issues.push({
                            type,
                            severity,
                            message,
                            line: lineNumber,
                            code: match[0],
                            suggestion: this.getSuggestion(type, this.currentLanguage)
                        });
                    });
                });

                return issues;
            }

            getSuggestion(issueType, language) {
                const suggestions = {
                    java: {
                        'Runtime Error': 'Add input validation: if (b != 0) before division',
                        'Array Access': 'Check bounds: if (index >= 0 && index < array.length)',
                        'NullPointer': 'Add null check: if (obj != null) before accessing',
                        'Error Handling': 'Log the exception or handle it appropriately'
                    },
                    python: {
                        'Runtime Error': 'Use try-except block or validate input before division',
                        'Array Access': 'Check list length: if index < len(list)',
                        'Error Handling': 'Specify exception type and handle appropriately'
                    },
                    javascript: {
                        'TypeError': 'Add null/undefined checks before property access',
                        'Comparison': 'Use strict equality (===) for better type safety'
                    },
                    cpp: {
                        'Runtime Error': 'Add input validation before division operation',
                        'Array Access': 'Use bounds checking or safer containers like std::vector',
                        'Memory': 'Use smart pointers (std::unique_ptr, std::shared_ptr)'
                    }
                };

                return suggestions[language]?.[issueType] || 'Review this code section for potential issues';
            }

            async generateAIInsights(issues, similarPatterns) {
                const insights = [];
                if (issues.length > 0) {
                    const highSeverityCount = issues.filter(i => i.severity === 'High').length;
                    const mediumSeverityCount = issues.filter(i => i.severity === 'Medium').length;
                    
                    insights.push({
                        title: `📊 Issue Analysis`,
                        content: `Found ${issues.length} potential issues: ${highSeverityCount} high priority, ${mediumSeverityCount} medium priority`
                    });
                }
                if (similarPatterns.length > 0) {
                    insights.push({
                        title: `🔍 Pattern Recognition`,
                        content: `Found ${similarPatterns.length} similar code patterns in knowledge base with known solutions`
                    });
                }
                insights.push({
                    title: `🎯 AI Recommendations`,
                    content: `Consider implementing defensive programming practices and input validation for better code reliability`
                });

                return insights;
            }

            displayDebugResults(issues, insights) {
                const output = document.getElementById('debugOutput');
                let result = `🔍 AI Code Analysis Complete\n`;
                result += `Language: ${this.currentLanguage.toUpperCase()}\n`;
                result += `Analysis Time: ${new Date().toLocaleTimeString()}\n\n`;

                if (issues.length === 0) {
                    result += `✅ No obvious issues detected!\n`;
                    result += `Code appears to follow good practices.\n\n`;
                } else {
                    result += `⚠️  Found ${issues.length} potential issues:\n\n`;
                    
                    issues.forEach((issue, index) => {
                        const severityIcon = issue.severity === 'High' ? '🔴' : 
                                           issue.severity === 'Medium' ? '🟡' : '🟢';
                        
                        result += `${index + 1}. ${severityIcon} ${issue.type} (Line ${issue.line})\n`;
                        result += `   Issue: ${issue.message}\n`;
                        result += `   Code: "${issue.code}"\n`;
                        result += `   Fix: ${issue.suggestion}\n\n`;
                    });
                }

                result += `🤖 AI Agent Status: Active\n`;
                result += `📚 Vector Store: Connected\n`;
                result += `🔗 Knowledge Base: ${Math.floor(Math.random() * 1000) + 9000}+ patterns analyzed\n`;

                output.textContent = result;
            }

            updateAIInsights(insights) {
                const container = document.getElementById('aiInsights');
                container.innerHTML = '';
                
                insights.forEach(insight => {
                    const item = document.createElement('div');
                    item.className = 'insight-item';
                    item.innerHTML = `
                        <h4>${insight.title}</h4>
                        <p>${insight.content}</p>
                    `;
                    container.appendChild(item);
                });
            }

            async explainIssues() {
                const code = document.getElementById('codeInput').value;
                const output = document.getElementById('debugOutput');
                
                if (!code.trim()) {
                    this.showError('Please enter some code first.');
                    return;
                }

                this.showLoading(output);
                await this.delay(1500);

                let explanation = `🧠 AI Code Explanation\n\n`;
                explanation += `Analyzing ${this.currentLanguage} code for educational purposes...\n\n`;
                
                // Simulate AI explanation based on language
                const explanations = {
                    java: `This Java code demonstrates several common programming pitfalls:

1. Division by Zero: The divide() method doesn't check if the divisor is zero
2. Array Bounds: getArrayElement() doesn't validate the index parameter
3. Exception Handling: No try-catch blocks for runtime exceptions

Best Practices:
- Always validate input parameters
- Use defensive programming techniques
- Implement proper error handling
- Consider using Optional<T> for nullable returns`,

                    python: `This Python code shows typical runtime errors:

1. ZeroDivisionError: Division by zero will raise an exception
2. IndexError: Accessing list index beyond bounds
3. NameError: Using undefined variables

Python Best Practices:
- Use try-except blocks for error handling
- Validate inputs before operations
- Use 'in' operator to check list bounds
- Initialize all variables before use`,

                    javascript: `This JavaScript code demonstrates common issues:

1. Division by zero returns Infinity (not an error)
2. Array access beyond bounds returns undefined
3. Null/undefined property access throws TypeError

JavaScript Best Practices:
- Use strict mode ('use strict')
- Check for null/undefined before property access
- Use optional chaining (?.) operator
- Implement proper error boundaries`,

                    cpp: `This C++ code shows memory and bounds issues:

1. Division by zero causes undefined behavior
2. Vector access without bounds checking
3. Potential buffer overflow vulnerabilities

C++ Best Practices:
- Use at() method for bounds-checked access
- Implement RAII (Resource Acquisition Is Initialization)
- Use smart pointers for memory management
- Always validate array/vector indices`
                };

                explanation += explanations[this.currentLanguage] || explanations.java;
                
                output.textContent = explanation;
                this.showSuccess('Code explanation generated successfully!');
            }

            clearAll() {
                document.getElementById('codeInput').value = '';
                document.getElementById('debugOutput').textContent = `AI Code Debugger cleared!

Ready for new code analysis...

🔧 Available Features:
- Multi-language support (Java, Python, JavaScript, C++)
- AI-powered static analysis
- Vector store pattern matching
- Real-time debugging suggestions
- Educational explanations`;
                
                document.getElementById('aiInsights').innerHTML = `
                    <div class="insight-item">
                        <h4>🎯 Ready for Analysis</h4>
                        <p>Enter your code and click Debug to start AI-powered analysis.</p>
                    </div>
                `;
            }

            showLoading(element) {
                element.innerHTML = `
                    <div class="loading">
                        <div class="spinner"></div>
                        <span>AI agent analyzing code...</span>
                    </div>
                `;
            }

            showError(message) {
                const container = document.querySelector('.container');
                const error = document.createElement('div');
                error.className = 'error';
                error.textContent = message;
                container.insertBefore(error, container.firstChild);
                setTimeout(() => error.remove(), 5000);
            }

            showSuccess(message) {
                const container = document.querySelector('.container');
                const success = document.createElement('div');
                success.className = 'success';
                success.textContent = message;
                container.insertBefore(success, container.firstChild);
                setTimeout(() => success.remove(), 3000);
            }

            delay(ms) {
                return new Promise(resolve => setTimeout(resolve, ms));
            }
        }

        // Simulated Vector Store for pattern matching
        class SimulatedVectorStore {
            constructor() {
                this.patterns = this.initializePatterns();
            }

            initializePatterns() {
                return [
                    { id: 1, pattern: 'division_by_zero', language: 'java', similarity: 0.95 },
                    { id: 2, pattern: 'array_bounds', language: 'java', similarity: 0.87 },
                    { id: 3, pattern: 'null_pointer', language: 'java', similarity: 0.92 },
                    { id: 4, pattern: 'index_error', language: 'python', similarity: 0.89 },
                    { id: 5, pattern: 'undefined_access', language: 'javascript', similarity: 0.84 }
                ];
            }

            async findSimilarPatterns(code, language) {
                // Simulate vector similarity search
                await this.delay(500);
                return this.patterns
                    .filter(p => p.language === language)
                    .slice(0, 3);
            }

            delay(ms) {
                return new Promise(resolve => setTimeout(resolve, ms));
            }
        }

        // Debugging Knowledge Base
        class DebuggingKnowledgeBase {
            constructor() {
                this.solutions = {
                    division_by_zero: 'Implement input validation before division operations',
                    array_bounds: 'Always check array bounds before accessing elements',
                    null_pointer: 'Use null checks or Optional types',
                    memory_leak: 'Implement proper resource management with RAII'
                };
            }

            getSolution(patternId) {
                return this.solutions[patternId] || 'Consult documentation for best practices';
            }
        }

        // Initialize the AI Code Debugger
        document.addEventListener('DOMContentLoaded', () => {
            const debugger = new AICodeDebugger();
            console.log('AI Code Debugger initialized successfully!');
        });

        // Additional utility functions for enhanced AI integration
        class RAGSystem {
            constructor() {
                this.documentStore = new Map();
                this.embeddings = new Map();
                this.initializeKnowledgeBase();
            }

            initializeKnowledgeBase() {
                // Simulate loading debugging documentation
                const docs = [
                    {
                        id: 'java_exceptions',
                        content: 'Java exception handling best practices and common patterns',
                        category: 'error_handling'
                    },
                    {
                        id: 'memory_management',
                        content: 'Memory leak detection and prevention strategies',
                        category: 'performance'
                    },
                    {
                        id: 'code_patterns',
                        content: 'Anti-patterns and their solutions in modern programming',
                        category: 'design_patterns'
                    }
                ];

                docs.forEach(doc => {
                    this.documentStore.set(doc.id, doc);
                    // Simulate embedding generation
                    this.embeddings.set(doc.id, this.generateEmbedding(doc.content));
                });
            }

            generateEmbedding(text) {
                // Simulate text embedding (normally done by AI model)
                return Array.from({length: 384}, () => Math.random() - 0.5);
            }

            async retrieveRelevantDocs(query, topK = 3) {
                // Simulate semantic search
                const queryEmbedding = this.generateEmbedding(query);
                const similarities = [];

                for (const [docId, embedding] of this.embeddings) {
                    const similarity = this.cosineSimilarity(queryEmbedding, embedding);
                    similarities.push({ docId, similarity });
                }

                return similarities
                    .sort((a, b) => b.similarity - a.similarity)
                    .slice(0, topK)
                    .map(item => this.documentStore.get(item.docId));
            }

            cosineSimilarity(a, b) {
                const dotProduct = a.reduce((sum, val, i) => sum + val * b[i], 0);
                const magnitudeA = Math.sqrt(a.reduce((sum, val) => sum + val * val, 0));
                const magnitudeB = Math.sqrt(b.reduce((sum, val) => sum + val * val, 0));
                return dotProduct / (magnitudeA * magnitudeB);
            }
        }

        // Enhanced AI Agent with LangChain-like capabilities
        class EnhancedAIAgent {
            constructor() {
                this.ragSystem = new RAGSystem();
                this.conversationHistory = [];
                this.tools = this.initializeTools();
            }

            initializeTools() {
                return {
                    codeAnalyzer: this.analyzeCodeStructure.bind(this),
                    bugDetector: this.detectCommonBugs.bind(this),
                    solutionGenerator: this.generateSolutions.bind(this),
                    testGenerator: this.generateTestCases.bind(this)
                };
            }

            async analyzeCodeStructure(code, language) {
                // Simulate advanced code structure analysis
                const analysis = {
                    complexity: this.calculateComplexity(code),
                    maintainability: this.assessMaintainability(code),
                    testability: this.assessTestability(code),
                    security: this.checkSecurityIssues(code, language)
                };

                return analysis;
            }

            calculateComplexity(code) {
                // Simplified cyclomatic complexity calculation
                const controlStructures = (code.match(/\b(if|while|for|switch|catch)\b/g) || []).length;
                return Math.min(controlStructures + 1, 10);
            }

            assessMaintainability(code) {
                const lines = code.split('\n').length;
                const comments = (code.match(/\/\*[\s\S]*?\*\/|\/\/.*$/gm) || []).length;
                const commentRatio = comments / lines;
                
                return {
                    score: Math.round((commentRatio * 40 + (lines < 100 ? 60 : 40)) * 100) / 100,
                    factors: {
                        length: lines < 100 ? 'Good' : 'Consider refactoring',
                        documentation: commentRatio > 0.1 ? 'Well documented' : 'Needs more comments'
                    }
                };
            }

            assessTestability(code) {
                const hasPublicMethods = /public\s+\w+\s+\w+\s*\(/.test(code);
                const hasDependencies = /new\s+\w+\s*\(/.test(code);
                
                return {
                    score: hasPublicMethods ? (hasDependencies ? 0.6 : 0.9) : 0.3,
                    suggestions: [
                        !hasPublicMethods ? 'Add public methods for testing' : null,
                        hasDependencies ? 'Consider dependency injection' : null
                    ].filter(Boolean)
                };
            }

            checkSecurityIssues(code, language) {
                const issues = [];
                
                // Common security patterns
                const securityPatterns = {
                    java: [
                        { pattern: /System\.out\.print/, message: 'Avoid logging sensitive information' },
                        { pattern: /Runtime\.getRuntime\.exec/, message: 'Command injection vulnerability' }
                    ],
                    javascript: [
                        { pattern: /eval\s*\(/, message: 'eval() can lead to code injection' },
                        { pattern: /innerHTML\s*=/, message: 'Potential XSS vulnerability' }
                    ]
                };

                const patterns = securityPatterns[language] || [];
                patterns.forEach(({ pattern, message }) => {
                    if (pattern.test(code)) {
                        issues.push(message);
                    }
                });

                return {
                    issues,
                    score: Math.max(0, 1 - issues.length * 0.2)
                };
            }

            async detectCommonBugs(code, language) {
                // Enhanced bug detection with ML-like patterns
                const bugs = [];
                
                // Simulate AI-powered bug detection
                const bugPatterns = await this.ragSystem.retrieveRelevantDocs(`${language} common bugs`);
                
                // Resource leak detection
                if (language === 'java' && /new\s+File/.test(code) && !/\.close\(\)/.test(code)) {
                    bugs.push({
                        type: 'Resource Leak',
                        severity: 'High',
                        message: 'File resources should be closed properly',
                        suggestion: 'Use try-with-resources statement'
                    });
                }

                // Infinite loop detection
                if (/while\s*\(\s*true\s*\)/.test(code) && !/break/.test(code)) {
                    bugs.push({
                        type: 'Infinite Loop',
                        severity: 'High',
                        message: 'Potential infinite loop detected',
                        suggestion: 'Add break condition or proper exit strategy'
                    });
                }

                return bugs;
            }

            async generateSolutions(issues) {
                const solutions = [];
                
                for (const issue of issues) {
                    const relevantDocs = await this.ragSystem.retrieveRelevantDocs(issue.type);
                    
                    solutions.push({
                        issue: issue.type,
                        solution: this.generateSolutionCode(issue),
                        explanation: `Based on ${relevantDocs.length} similar cases in knowledge base`,
                        confidence: Math.random() * 0.3 + 0.7 // 70-100% confidence
                    });
                }

                return solutions;
            }

            generateSolutionCode(issue) {
                const solutions = {
                    'Runtime Error': `
// Before fix:
// return a / b;

// After fix:
if (b == 0) {
    throw new IllegalArgumentException("Division by zero");
}
return a / b;`,
                    
                    'Array Access': `
// Before fix:
// return numbers[index];

// After fix:
if (index < 0 || index >= numbers.length) {
    throw new IndexOutOfBoundsException("Index: " + index);
}
return numbers[index];`,
                    
                    'Resource Leak': `
// Before fix:
// FileReader fr = new FileReader("file.txt");

// After fix:
try (FileReader fr = new FileReader("file.txt")) {
    // Use the resource
} // Automatically closed`
                };

                return solutions[issue.type] || '// Solution not available for this issue type';
            }

            async generateTestCases(code, language) {
                // AI-powered test case generation
                const testCases = [];

                if (language === 'java') {
                    testCases.push(`
@Test
public void testDivideByZero() {
    Calculator calc = new Calculator();
    assertThrows(IllegalArgumentException.class, () -> {
        calc.divide(10, 0);
    });
}

@Test
public void testValidDivision() {
    Calculator calc = new Calculator();
    assertEquals(5.0, calc.divide(10, 2), 0.001);
}

@Test
public void testArrayBounds() {
    Calculator calc = new Calculator();
    assertThrows(IndexOutOfBoundsException.class, () -> {
        calc.getArrayElement(10);
    });
}`);
                }

                return testCases;
            }
        }

        // API Integration Simulator
        class APIIntegration {
            constructor() {
                this.baseURL = 'https://api.codeanalyzer.ai/v1';
                this.apiKey = 'demo-key-for-hackathon';
            }

            async analyzeCodeWithAI(code, language) {
                // Simulate API call to external AI service
                try {
                    const response = await this.simulateAPICall({
                        endpoint: '/analyze',
                        method: 'POST',
                        data: {
                            code: code,
                            language: language,
                            features: ['bug_detection', 'performance', 'security']
                        }
                    });

                    return response;
                } catch (error) {
                    console.error('API Error:', error);
                    return this.fallbackAnalysis(code, language);
                }
            }

            async simulateAPICall(config) {
                // Simulate network delay
                await new Promise(resolve => setTimeout(resolve, 1000 + Math.random() * 1000));
                
                // Simulate occasional API failures
                if (Math.random() < 0.1) {
                    throw new Error('API rate limit exceeded');
                }

                return {
                    status: 'success',
                    analysis: {
                        bugs_found: Math.floor(Math.random() * 5),
                        performance_score: Math.random() * 100,
                        security_score: Math.random() * 100,
                        suggestions: [
                            'Consider using more descriptive variable names',
                            'Add input validation for better error handling',
                            'Implement proper logging mechanisms'
                        ]
                    },
                    processing_time: Math.random() * 2000 + 500
                };
            }

            fallbackAnalysis(code, language) {
                return {
                    status: 'fallback',
                    analysis: {
                        bugs_found: 2,
                        performance_score: 75,
                        security_score: 80,
                        suggestions: [
                            'Using local analysis due to API unavailability',
                            'Consider running full analysis when API is available'
                        ]
                    }
                };
            }
        }

        // Initialize enhanced systems
        window.enhancedAI = new EnhancedAIAgent();
        window.apiIntegration = new APIIntegration();
    </script>
</body>
</html>
