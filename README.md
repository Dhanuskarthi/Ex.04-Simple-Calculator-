# Ex04 Simple Calculator - React Project
## Date:24/10/25

## AIM
To  develop a Simple Calculator using React.js with clean and responsive design, ensuring a smooth user experience across different screen sizes.

## ALGORITHM
### STEP 1
Create a React App.

### STEP 2
Open a terminal and run:
  <ul><li>npx create-react-app simple-calculator</li>
  <li>cd simple-calculator</li>
  <li>npm start</li></ul>

### STEP 3
Inside the src/ folder, create a new file Calculator.js and define the basic structure.

### STEP 4
Plan the UI: Display screen, number buttons (0-9), operators (+, -, *, /), clear (C), and equal (=).

### STEP 5
Create a new file Calculator.css in src/ and add the styling.

### STEP 6
Open src/App.js and modify it.

### STEP 7
Start the development server.
  npm start

### STEP 8
Open http://localhost:3000/ in the browser.

### STEP 9
Test the calculator by entering numbers and operations.

### STEP 10
Fix styling issues and refine content placement.

### STEP 11
Deploy the website.

### STEP 12
Upload to GitHub Pages for free hosting.

## PROGRAM
## app.jsx
```
import React from "react";
import Calculator from "./Calculator";

export default function App() {
  return (
    <div className="app-root">
      <h1 className="title">Simple Calculator</h1>
      <Calculator />
    </div>
  );
}

```
## calculator.jsx
```
import React, { useState } from "react";

const BUTTONS = [
  "7", "8", "9", "/",
  "4", "5", "6", "*",
  "1", "2", "3", "-",
  "0", ".", "=", "+",
  "C"
];

export default function Calculator() {
  const [input, setInput] = useState("");

  const handleClick = (value) => {
    if (value === "C") {
      setInput("");
      return;
    }

    if (value === "=") {
      if (!input) return;
      try {
        // Simple evaluation. For complex needs, replace with a proper parser.
        // Remove leading zeros that could confuse eval (keeps decimal numbers OK)
        const result = eval(input);
        setInput(String(result));
      } catch {
        setInput("Error");
        setTimeout(() => setInput(""), 1200);
      }
      return;
    }

    // Prevent two operator chars in a row (basic hygiene)
    const lastChar = input.slice(-1);
    const operators = ["/", "*", "-", "+", "."];
    if (operators.includes(value) && operators.includes(lastChar)) {
      // replace last operator with the new one (except dot)
      if (value === ".") {
        // allow 0. etc — if lastChar is operator other than dot, append "0."
        setInput(input + "0.");
        return;
      }
      setInput(input.slice(0, -1) + value);
      return;
    }

    setInput(input + value);
  };

  return (
    <div className="calculator">
      <div className="display" role="status" aria-live="polite">
        {input || "0"}
      </div>

      <div className="buttons">
        {BUTTONS.map((b) => (
          <button
            key={b}
            onClick={() => handleClick(b)}
            className={`btn ${b === "=" ? "btn-equal" : b === "C" ? "btn-clear" : ""}`}
            type="button"
          >
            {b}
          </button>
        ))}
      </div>
    </div>
  );
}

```
## index.css
```
:root{
  --bg: #0f1720;
  --card: #0f1725;
  --muted: #a3b0bf;
  --accent: #00c6ff;
  --accent-dark: #00a6dd;
  --danger: #ff5a5a;
  --btn: #1f2933;
}

* { box-sizing: border-box; }
html,body,#root { height: 100%; margin: 0; font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background: linear-gradient(180deg, #071023 0%, #0f1720 100%); color: #e6eef6; }

.app-root {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 18px;
  justify-content: center;
  align-items: center;
  padding: 24px;
}

.title {
  font-size: 1.6rem;
  margin: 0;
  color: var(--accent);
  text-shadow: 0 4px 18px rgba(0,198,255,0.12);
}

/* Calculator card */
.calculator {
  width: 360px;
  max-width: 96%;
  background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
  border-radius: 14px;
  padding: 18px;
  box-shadow: 0 10px 30px rgba(2,6,23,0.6);
  border: 1px solid rgba(255,255,255,0.02);
}

/* Display screen */
.display {
  background: rgba(2,6,23,0.6);
  padding: 14px;
  border-radius: 10px;
  font-family: "Roboto Mono", ui-monospace, SFMono-Regular, Menlo, Monaco, "Courier New", monospace;
  font-size: 1.8rem;
  text-align: right;
  min-height: 56px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  overflow-x: auto;
  scrollbar-width: thin;
  color: #eaf6ff;
}

/* Buttons grid */
.buttons {
  margin-top: 14px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

/* Buttons */
.btn {
  background: var(--btn);
  border: none;
  border-radius: 10px;
  padding: 14px 10px;
  font-size: 1.05rem;
  color: #e6eef6;
  cursor: pointer;
  transition: transform 0.12s ease, box-shadow 0.12s ease, background 0.12s ease;
  box-shadow: 0 6px 14px rgba(2,6,23,0.5);
}

.btn:active { transform: translateY(1px) scale(0.998); }

.btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 16px 30px rgba(2,6,23,0.55);
}

/* special buttons */
.btn-equal {
  background: linear-gradient(180deg, var(--accent) 0%, var(--accent-dark) 100%);
  color: #001217;
  font-weight: 700;
  grid-column: span 1;
}

.btn-clear {
  background: linear-gradient(180deg, #ff7b7b 0%, var(--danger) 100%);
  color: #140808;
  font-weight: 600;
}

/* Responsive */
@media (max-width: 420px) {
  .title { font-size: 1.25rem; }
  .calculator { width: 100%; padding: 14px; }
  .display { font-size: 1.4rem; min-height: 48px; padding: 10px; }
  .btn { padding: 12px 8px; font-size: 1rem; }
}
```
## OUTPUT

<img width="1920" height="1200" alt="Screenshot 2025-10-24 193315" src="https://github.com/user-attachments/assets/80c4d147-248e-44af-abe8-02d1b8c7b6ca" />


## RESULT
The program for developing a simple calculator in React.js is executed successfully.
