---
# OPERATING SYSTEM LAB
### Experiment No.: 02
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 02                    |

---

## TITLE: Shell Scripting — Variables, Loops, Conditions, and Functions

---

## AIM

To write and execute shell scripts demonstrating the use of variables, conditional statements, loops, and functions in Bash shell scripting.

---

## THEORY

A **Shell Script** is a text file containing a sequence of shell commands that are executed by the shell interpreter. It automates repetitive tasks and is widely used in system administration.

### Key Concepts:

**Shebang (`#!/bin/bash`):**  
The first line of every shell script. It tells the OS which interpreter to use.

**Variables:**
```bash
name="Pranay"        # String variable
num=10               # Integer variable
echo $name           # Access variable with $
```

**Conditional Statements:**
```bash
if [ condition ]; then
    # statements
elif [ condition ]; then
    # statements
else
    # statements
fi
```

**Comparison Operators:**
| Operator | Meaning               |
|----------|-----------------------|
| `-eq`    | Equal to              |
| `-ne`    | Not equal to          |
| `-lt`    | Less than             |
| `-le`    | Less than or equal    |
| `-gt`    | Greater than          |
| `-ge`    | Greater than or equal |

**Loops:**
```bash
# For loop
for i in 1 2 3 4 5; do
    echo $i
done

# While loop
while [ $i -le 10 ]; do
    echo $i
    i=$((i+1))
done
```

**Functions:**
```bash
function greet() {
    echo "Hello, $1!"
}
greet "Pranay"
```

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux / Any Linux Distribution
- **Software:** Terminal (Bash Shell), Text Editor (nano / vim / VS Code)
- **RAM:** Minimum 2 GB

---

## PROCEDURE

### Program 1: Hello World and Variables
```bash
#!/bin/bash
# Script: variables.sh
# Author: Pranay K Gajbhiye

name="Pranay K Gajbhiye"
branch="CSE"
semester=4

echo "==============================="
echo "  Student Information"
echo "==============================="
echo "Name     : $name"
echo "Branch   : $branch"
echo "Semester : $semester"
echo "==============================="
```

**Run:**
```bash
chmod +x variables.sh
./variables.sh
```

---

### Program 2: Check Even or Odd
```bash
#!/bin/bash
# Script: even_odd.sh

echo "Enter a number:"
read num

if [ $((num % 2)) -eq 0 ]; then
    echo "$num is EVEN"
else
    echo "$num is ODD"
fi
```

---

### Program 3: Grade Calculator
```bash
#!/bin/bash
# Script: grade.sh

echo "Enter marks (out of 100):"
read marks

if [ $marks -ge 90 ]; then
    echo "Grade: O (Outstanding)"
elif [ $marks -ge 75 ]; then
    echo "Grade: A (Excellent)"
elif [ $marks -ge 60 ]; then
    echo "Grade: B (Good)"
elif [ $marks -ge 50 ]; then
    echo "Grade: C (Average)"
elif [ $marks -ge 40 ]; then
    echo "Grade: D (Pass)"
else
    echo "Grade: F (Fail)"
fi
```

---

### Program 4: Factorial using Loop
```bash
#!/bin/bash
# Script: factorial.sh

echo "Enter a number:"
read n

fact=1
i=1

while [ $i -le $n ]; do
    fact=$((fact * i))
    i=$((i + 1))
done

echo "Factorial of $n = $fact"
```

---

### Program 5: Multiplication Table using For Loop
```bash
#!/bin/bash
# Script: table.sh

echo "Enter a number:"
read n

echo "Multiplication Table of $n:"
echo "================================"
for i in $(seq 1 10); do
    result=$((n * i))
    echo "$n x $i = $result"
done
echo "================================"
```

---

### Program 6: Function — Find Largest of Three Numbers
```bash
#!/bin/bash
# Script: largest.sh

function findLargest() {
    a=$1
    b=$2
    c=$3

    if [ $a -ge $b ] && [ $a -ge $c ]; then
        echo "Largest number is: $a"
    elif [ $b -ge $a ] && [ $b -ge $c ]; then
        echo "Largest number is: $b"
    else
        echo "Largest number is: $c"
    fi
}

echo "Enter three numbers:"
read x y z
findLargest $x $y $z
```

---

## OUTPUT / OBSERVATIONS

**Program 1 — Variables:**
```
===============================
  Student Information
===============================
Name     : Pranay K Gajbhiye
Branch   : CSE
Semester : 4
===============================
```

**Program 2 — Even/Odd:**
```
Enter a number:
7
7 is ODD
```

**Program 3 — Grade:**
```
Enter marks (out of 100):
82
Grade: A (Excellent)
```

**Program 4 — Factorial:**
```
Enter a number:
5
Factorial of 5 = 120
```

**Program 5 — Table:**
```
Enter a number:
7
Multiplication Table of 7:
================================
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
7 x 4 = 28
7 x 5 = 35
7 x 6 = 42
7 x 7 = 49
7 x 8 = 56
7 x 9 = 63
7 x 10 = 70
================================
```

**Program 6 — Largest:**
```
Enter three numbers:
45 78 32
Largest number is: 78
```

---

## RESULT

Shell scripts were successfully written and executed. The programs demonstrated use of variables, conditional statements (`if-elif-else`), loops (`while`, `for`), and functions in Bash. All programs produced the correct and expected output.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 02*
