# Experiment 04 — C Programs Using Vi Editor

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To write and execute C programs using the Vi editor in Linux:
- (i) Print "Hello, World!"
- (ii) Perform basic arithmetic operations

---

## Theory

### Vi Editor

**Vi** (Visual Editor) is a text editor built into almost every Unix/Linux system. It is lightweight, powerful, and works entirely from the keyboard. Its improved version, **Vim** (Vi Improved), adds syntax highlighting and additional features.

### Vi Editor Modes

| Mode          | How to Enter        | Purpose                              |
|---------------|---------------------|--------------------------------------|
| Command Mode  | Default / `Esc`     | Navigate, delete, copy, paste        |
| Insert Mode   | `i`, `a`, `o`       | Type and edit text                   |
| Visual Mode   | `v`                 | Select text                          |
| Ex Mode       | `:`                 | Save, quit, search/replace           |

### Common Vi Commands

| Command     | Action                             |
|-------------|------------------------------------|
| `i`         | Enter insert mode before cursor    |
| `Esc`       | Return to command mode             |
| `:w`        | Save file                          |
| `:q`        | Quit Vi                            |
| `:wq`       | Save and quit                      |
| `:q!`       | Quit without saving                |
| `dd`        | Delete current line                |
| `yy`        | Copy (yank) current line           |
| `p`         | Paste                              |
| `/word`     | Search for "word"                  |
| `u`         | Undo last action                   |

### GCC Compiler

**GCC** (GNU Compiler Collection) is used to compile C programs on Linux.

**Compilation command:**
```bash
gcc filename.c -o output_name
```

**Execution:**
```bash
./output_name
```

---

## Apparatus / Requirements

| Component       | Specification                        |
|-----------------|--------------------------------------|
| Computer System | Any PC with minimum 2 GB RAM        |
| Operating System| Linux (Ubuntu 20.04 or later)        |
| Editor          | Vi / Vim                             |
| Compiler        | GCC (GNU C Compiler)                 |

---

## Procedure

1. Open the terminal.
2. Launch Vi editor with the filename: `vi hello.c`
3. Press `i` to enter insert mode.
4. Type the C program.
5. Press `Esc` to exit insert mode.
6. Type `:wq` and press Enter to save and exit.
7. Compile: `gcc hello.c -o hello`
8. Execute: `./hello`
9. Observe the output.
10. Repeat the same steps for the arithmetic program.

---

## Part (i) — Hello World Program

### Program: `hello.c`

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

### Steps in Vi Editor

```
$ vi hello.c

(Vi opens — you are in Command Mode)
Press: i           → Enter Insert Mode
Type the program above
Press: Esc         → Return to Command Mode
Type:  :wq         → Save and Quit
```

### Compilation and Execution

```bash
$ gcc hello.c -o hello
$ ./hello
```

### Output

```
Hello, World!
```

---

## Part (ii) — Arithmetic Operations Program

### Program: `arithmetic.c`

```c
#include <stdio.h>

int main() {
    int a, b;
    int sum, difference, product;
    float quotient, modulus;

    printf("Enter two integers: ");
    scanf("%d %d", &a, &b);

    sum        = a + b;
    difference = a - b;
    product    = a * b;

    printf("\n--- Arithmetic Operations ---\n");
    printf("Addition       : %d + %d = %d\n", a, b, sum);
    printf("Subtraction    : %d - %d = %d\n", a, b, difference);
    printf("Multiplication : %d * %d = %d\n", a, b, product);

    if (b != 0) {
        quotient = (float)a / b;
        modulus  = a % b;
        printf("Division       : %d / %d = %.2f\n", a, b, quotient);
        printf("Modulus        : %d %% %d = %.0f\n", a, b, modulus);
    } else {
        printf("Division       : Cannot divide by zero\n");
        printf("Modulus        : Cannot divide by zero\n");
    }

    return 0;
}
```

### Steps in Vi Editor

```
$ vi arithmetic.c

Press: i           → Enter Insert Mode
Type the program above
Press: Esc         → Return to Command Mode
Type:  :wq         → Save and Quit
```

### Compilation and Execution

```bash
$ gcc arithmetic.c -o arithmetic
$ ./arithmetic
```

### Output

```
Enter two integers: 15 4

--- Arithmetic Operations ---
Addition       : 15 + 4 = 19
Subtraction    : 15 - 4 = 11
Multiplication : 15 * 4 = 60
Division       : 15 / 4 = 3.75
Modulus        : 15 % 4 = 3
```

**Test with b = 0:**

```
Enter two integers: 10 0

--- Arithmetic Operations ---
Addition       : 10 + 0 = 10
Subtraction    : 10 - 0 = 10
Multiplication : 10 * 0 = 0
Division       : Cannot divide by zero
Modulus        : Cannot divide by zero
```

---

## Result

Two C programs were successfully written using the Vi editor in Linux:
- (i) "Hello, World!" was printed to the terminal.
- (ii) Arithmetic operations (addition, subtraction, multiplication, division, and modulus) were performed on two user-input integers.

Both programs compiled and executed without errors using GCC.
