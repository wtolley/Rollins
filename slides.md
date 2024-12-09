---
theme: seriph
background: https://cover.sli.dev
class: 'text-center'
highlighter: shiki
info: |
  ## Introduction to Reverse Engineering

  Learn more at [Sli.dev](https://sli.dev)
transition: slide-left
title: "Introduction to Reverse Engineering"
mdc: true
---

# Introduction to Reverse Engineering

---

# Introduction to Reverse Engineering

## Welcome and Overview
- Welcome to the session on Reverse Engineering!
- Today, we'll explore the exciting world of disassembling, analyzing, and understanding both ARM and x86 architectures.
- Key focus: Practical reverse engineering using GDB.

---

## What is Reverse Engineering?
- **Definition**: Reverse engineering involves taking apart an object or system to see how it works in order to duplicate or enhance the object.
- **Applications**:
  - Software security analysis.
  - Malware dissection.
  - System enhancements.

---

## Importance in Cybersecurity and Software Development
- **Cybersecurity**: Understanding potential vulnerabilities and building more secure systems.
- **Software Development**: Learning from existing systems to create more efficient and robust applications.
- **Educational Value**: Sharpens problem-solving and analytical skills.

---

## Goals for Today's Lecture
- Compare ARM and x86 architectures.
- Dive into practical reverse engineering strategies using GDB.
- Hands-on dynamic analysis with real-world examples.

---

# Section II: ARM vs. x86: Understanding the Basics

## Architecture Overview
- **ARM**: A RISC (Reduced Instruction Set Computing) architecture known for its simplicity and power efficiency. Widely used in mobile and embedded devices.
- **x86**: A CISC (Complex Instruction Set Computing) architecture with a rich set of instructions. Dominant in desktop and server environments.

---

## Instruction Set Complexity
- **ARM**: Streamlined, fixed-length instructions that facilitate decoding and execution. 
- **x86**: Variable-length instructions with a complex encoding scheme, allowing for powerful, though sometimes intricate, operations.

---

## Register Set
- **ARM**: Typically has more general-purpose registers than x86, reducing the need for frequent memory access.
- **x86**: Limited number of general-purpose registers, which can lead to more complex memory management in software.

---

## Register Usage:
- **ARM**: More registers, less memory access.
- **x86**: Fewer registers, more complex code.

---

# Instruction Set and Register Comparison

| Architecture | Instruction Example    | General Purpose Registers |
|--------------|------------------------|---------------------------|
| ARM          | ADD R0, R1, R2         | 16 registers (R0-R15)     |
| x86          | ADD EAX, EBX           | 8 registers (EAX, EBX...) |

- **Simplification**: ARM uses straightforward instructions for efficiency.
- **Complexity**: x86 offers powerful instructions for diverse applications.

---

## Common Use Cases
- **ARM**: Preferred for applications where power efficiency and cost are critical, such as in smartphones, tablets, and other portable devices.
- **x86**: Favoured for performance-intensive applications like gaming, data processing, and enterprise software.

---

# Practical Example with Simple Operations

- **Objective**: Compare how ARM and x86 architectures handle basic operations.
- **Example Task**: Sum the elements of an integer array.
- **Languages**: ARM Assembly and x86 Assembly.

---

# Sum Array Function in C

```c
int sumArray(int arr[], int size) {
    int sum = 0;
    for (int i = 0; i < size; i++) {
        sum += arr[i];
    }
    return sum;
}
```

- **Function Overview:** Iterates through an array, accumulates the sum of its elements.

---


### Sum Array in ARM Assembly

# Sum Array in ARM Assembly

```ts
sumArray:
    MOV R2, #0               // Initialize sum to 0
    MOV R3, #0               // Initialize index i to 0
loop:
    CMP R3, R1               // Compare index i to size
    BGE end                  // If i >= size, branch to end
    LDR R4, [R0, R3, LSL #2] // Load arr[i] into R4 (multiply index by 4)
    ADD R2, R2, R4           // Add arr[i] to sum
    ADD R3, R3, #1           // Increment index i
    B loop                   // Repeat loop
end:
    MOV R0, R2               // Return sum in R0
    BX LR                    // Return from function
```

- **Explanation:** Utilizes registers for loop control and data access, minimizing memory interaction.

---


### Sum Array in x86 Assembly

# Sum Array in x86 Assembly

```ts
_sumArray:
    push ebp          // Save base pointer
    mov ebp, esp      // Set base pointer to stack pointer
    xor eax, eax      // Clear eax to use as sum
    xor ecx, ecx      // Clear ecx to use as index i
    jmp check         // Jump to condition check
loop:
    mov edx, [ebp+8]  // Move address of array into edx
    mov edx, [edx + ecx*4] // Load arr[i] into edx
    add eax, edx      // Add arr[i] to sum
    add ecx, 1        // Increment index i
check:
    cmp ecx, [ebp+12] // Compare index i to size
    jl loop           // Continue loop if i < size
    pop ebp           // Restore base pointer
    ret               // Return
```

- **Explanation:** Shows traditional use of the stack for function calls and loop control, with a focus on conditional jumps.

---

# Introduction to Reverse Engineering Tools and Techniques

## Understanding Basic Tools

- **Objdump for Static Analysis**:
  - **Usage**: `objdump -d <binary>` to disassemble executables.
  - **Purpose**: Inspects the static structure of binaries without executing them, displaying assembly code.
  - **Example**: Disassemble a simple C program to view its assembly instructions.

---

## Limitations of Static Analysis

- **Questions Raised**:
  - What if we want to see the program in action?
  - How does the binary behave during execution?
  - What happens to specific variables during runtime?

---

## Transition to Dynamic Analysis with GDB

- **Introducing GDB**:
  - **Dynamic Analysis**: GDB allows stepping through the program, observing behavior, and modifying states in real-time.
  - **Capabilities**:
    - Set breakpoints and watchpoints.
    - Monitor and change variable values.
    - Control program execution flow.

---

## Hands-On GDB Session

- **Practical Exercise**: Use GDB to dynamically analyze the same binary.
  - Set a breakpoint at the main function.
  - Watch how variables change throughout the program execution.
  - Modify values to see real-time effects and understand decision-making within the program.

---

# Comprehensive Guide to SSH (Secure Shell)

## Introduction to SSH
- **Purpose**: Securely connect and operate on a remote server using an encrypted connection.
- **Usage**: Commonly used for remote system management, file transfers, and operating network services.

---

## Preparing to Connect via SSH
- **Server Address**: `207.246.62.89`
- **Username**: Each participant has a unique username ranging from `user1` to `user40`.
- **Password**: Shared securely for this session.
- **Ensure**: Each user should ensure they have SSH client software installed (e.g., PuTTY for Windows, OpenSSH for Linux/Mac).

---

## SSH Command Syntax
- **Basic Command**: `ssh [username]@[server-address]`
- **Example**: `ssh user1@207.246.62.89`
- **Entering this command**: Prompts you to enter the password for the server access.

---

## Connecting to the Server
- **Step-by-Step**:
  1. Open your terminal or SSH client.
  2. Type the command: `ssh userX@207.246.62.89` (Replace `X` with your assigned number).
  3. When prompted, enter the password: `SuperSecretUnguessablePassword`.
  4. If successful, you will be logged into the server.

---

# Mastering GDB: Essential Commands

## Introduction to GDB
- **GDB (GNU Debugger)**: A powerful tool used to debug programs written in C, C++, and other languages.
- **Purpose**: Enables developers to examine what is happening inside a program as it executes, or to analyze what the program was doing at the moment it crashed.

## Starting GDB
- **Launch GDB**: `gdb [program]`
  - Load your program into GDB for debugging.
- **Example**: `gdb ./myprogram`

---

## Basic GDB Commands
- **Run the Program**: `run [arguments]`
  - Starts your program with any specified arguments.
- **Setting Breakpoints**: `break [location]`
  - Interrupts the program execution at a function or a specific line number.
  - **Examples**: `break main` or `break 24`
- **Stepping Through the Program**:
  - `next` (n): Execute the next line of the program, stepping over function calls.
  - `step` (s): Step into any function calls.

---

## Displaying Variable Values
- **Print Variable Values**: `print [variable]` or `p [variable]`
  - Displays the current value of a variable.
  - **Example**: `print x`
- **Examine Memory**: `x/[format] [address]`
  - Examines memory at a given address using a specified format.
  - **Formats**:
    - `x/gx $rsp`: Examine memory as giant-sized hex values at the stack pointer.
    - `x/10i $pc`: Display the next 10 instructions from the program counter.
  - **Usage**: Choose the number and format for the memory examination based on what you need to see (e.g., hexadecimal, instructions).

---

## Continuing and Controlling Execution
- **Continue Execution**: `continue` (c)
  - Resumes running the program until the next breakpoint or watchpoint.

## Exiting GDB
- **Quit GDB**: `quit` (q)
  - Exit the debugger.

## Additional Tips
- **List of Commands**: `help`
  - Provides a list of all available GDB commands and a brief description of their purposes.
- **Re-run Program**: `run`
  - Restart the program from the beginning within GDB.