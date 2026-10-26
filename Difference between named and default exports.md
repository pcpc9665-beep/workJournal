
The primary difference between **named exports** and **default exports** in JavaScript (ES6) lies in ==**how many items you can export per file** and **how they are imported** into another file==.

Here is a direct comparison of their differences:

|Feature|Named Exports|Default Exports|
|---|---|---|
|**Quantity per file**|**Multiple** distinct entities can be exported.|Only **one** primary entity per file.|
|**Import syntax**|Requires **curly braces `{ }`**.|Strictly **no curly braces**.|
|**Import naming**|Must match the **exact exported name** (unless aliased with `as`).|Can be imported using **any custom name**.|
|**Common Use Case**|Utility files, math libraries, or components packages.|A standalone React component or main configuration file.|

Named Exports Syntax & Example

Named exports force the importing file to use the exact names of the exported variables or functions.

javascript

```
// mathUtils.js (Exporting)
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// main.js (Importing)
import { add, subtract } from './mathUtils.js'; 
// To rename a named export, you must use the 'as' keyword:
// import { add as sum } from './mathUtils.js';
```



Default Exports Syntax & Example

Default exports designate one value as the fallback or primary export of the file. Because it is the only default, you can name it whatever you like when importing. 

javascript

```
// Calculator.js (Exporting)
const Calculator = () => { ... };
export default Calculator;

// main.js (Importing - notice no curly braces and flexible naming)
import MyCalculator from './Calculator.js'; 
import AlternateName from './Calculator.js'; // Both work perfectly
```



Combining Both

You can mix both types within a single file. When importing them together, the default export sits outside the curly braces, while the named exports remain inside them.

javascript

```
// components.js
export const ThemeContext = React.createContext(); // Named
export default function Dashboard() { ... }       // Default

// main.js
import Dashboard, { ThemeContext } from './components.js';
```

