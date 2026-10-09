
The core difference is that ==**compile time** is when a computer program's source code is translated into machine code or bytecode, while **runtime** is when that translated program is actively executing on the CPU==. 
Key Differences

| Feature             | Compile Time                                                                           | Runtime                                                                                    |
| ------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Definition**      | The phase when a compiler translates high-level code into executable or binary format. | The phase when the compiled program is loaded into memory and run by the computer.         |
| **When it happens** | Before the user runs the program.                                                      | After compilation, when the user launches and interacts with the program.                  |
| **Common Errors**   | Syntax errors, missing semicolons, missing brackets, or type mismatches.               | Division by zero, null pointer dereferences, array out-of-bounds, or memory leaks.         |
| **Error Detection** | Caught easily and directly pointed out by the compiler before the program builds.      | Harder to catch in advance; often triggered by specific data, user inputs, or logic flows. |

Detailed Breakdown

- **Compile Time:**
    - **What happens:** Tools (like `gcc`, `javac`, or Go compiler) scan your code to check grammar, rules, and data types.
    - **Result:** If there is a rule violation (such as a forgotten semicolon or passing a string into a function expecting a number), the compiler stops and reports a **compile-time error**, refusing to build the program. 

- **Runtime:**
    
    - **What happens:** The operating system or a virtual machine (like the JVM) loads the compiled program into memory and executes its instructions line by line.
    - **Result:** Even if the code compiles cleanly, unexpected conditions during execution—such as a user entering zero for a division operation or trying to read a file that does not exist—cause a **runtime error** (or exception).
    

Would you like to see an example of compile-time versus runtime behavior in a specific programming language like **Python, Java, or C++**?