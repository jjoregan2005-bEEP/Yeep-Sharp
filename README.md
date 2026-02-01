# Yeep-Sharp (ypsh)

Yeep-Sharp is a small interpreted scripting language implemented in Python. The interpreter lives in the `ypsh` file, and scripts use the `.ysh` extension.

This document explains the repository structure, how to run scripts, and provides a full language reference.

---

##  Repository Structure

```
.
├── install.sh     # Installation script (system-wide install)
├── LICENSE        # Project license
├── ypsh           # Yeep-Sharp interpreter (Python script)
├── test.ysh       # Example Yeep-Sharp script
├── test2.ysh      # Additional example script
```

.
├── LICENSE        # Project license
├── ypsh           # Yeep-Sharp interpreter (Python script)
├── test.ysh       # Example Yeep-Sharp script
├── test2.ysh      # Additional example script

````

---

##  Installation

Yeep-Sharp can be installed system-wide using the provided `install.sh` script.

```bash
chmod +x install.sh
sudo ./install.sh
````

The script:

* Installs Python 3
* Ensures `python` maps to `python3`
* Copies `ypsh` into `/bin/ypsh`

After installation, the interpreter can be run from anywhere.

---

## Running Yeep-Sharp

Once installed, run a script with:

```bash
ypsh test.ysh
```

You may also run it directly with Python:

```bash
python ypsh test.ysh
```

---

##  Language Overview

Yeep-Sharp is token-based and line-oriented. Execution is sequential unless redirected using jumps or calls.

### Token Types

* **Keyword** – built-in instructions (`var`, `print`, `add`, etc.)
* **Identifier** – variable or label names
* **Number** – numeric literals (stored as floats)
* **String** – text wrapped in double quotes
* **Label** – identifiers ending with `:`

---

##  Labels

Labels define jump targets.

```ysh
start:
print "Hello"
jmp start
```

Labels are resolved before execution begins.

---

##  Variables

### Declaration / Assignment

```ysh
var x 10
var name "John"
var y x
```

* Numbers are stored as floats
* Strings support escape sequences

### String Escape Sequences

| Escape | Meaning   |
| ------ | --------- |
| `\n`   | Newline   |
| `\t`   | Tab       |
| `\b`   | Backspace |
| `\\`   | Backslash |

---

##  Output

### `print`

Prints with a newline.

```ysh
print "Hello world"
print x
```

### `print!`

Prints without a newline.

```ysh
print! "Loading..."
```

---

##  Arithmetic Operations

All arithmetic operates on variables.

```ysh
add x 5
sub x 2
mul x 3
div x 4
```

The second argument may be a number or another variable.

---

##  String Concatenation

```ysh
concat greeting name
```

Appends the value of `name` to `greeting`.

---

## ⌨ Input

```ysh
inp x 0
```

* If the second argument is `< 1`, input is stored as a string
* Otherwise, input is converted to a float

---

##  Comparison

```ysh
cmp a b
```

Sets internal comparison flags:

* `gt` – greater than
* `lt` – less than
* `eq` – equal

These flags are used by conditional jumps.

---

##  Jumps

### Unconditional Jump

```ysh
jmp label
```

### Conditional Jumps

```ysh
gj label   # jump if greater
lj label   # jump if less
ej label   # jump if equal
```

A `cmp` instruction must be executed before conditional jumps.

---

## Call / Return

### Call

```ysh
call function_label
```

Stores the return position and jumps to the label.

### Return

```ysh
ret
```

Returns execution to the most recent `call`.

---

##  Arrays

### Create Array

```ysh
arryset myArray
```

### Add Element

```ysh
arryadd myArray value
```

### Remove Element by Index

```ysh
arrypop myArray index
```

### Fetch Element

```ysh
arryfet result myArray index
```

### Array Length

```ysh
arrylen lengthVar myArray
```

---

##  File I/O

### Read File

```ysh
read targetVar pathVar
```

Reads the contents of the file at `pathVar` into `targetVar`.

### Write File

```ysh
write dataVar pathVar
```

Overwrites the file at `pathVar` with `dataVar`.

---

##  System Commands

```ysh
system "ls"
system commandVar
```

Executes shell commands using the host operating system.

 **Warning:** This uses `os.system` and executes commands directly.

---

##  No Operation

```ysh
nop
```

Does nothing.

---

## Example Script

```ysh
var x 5
var y 10

cmp x y
lj smaller

print "x is bigger"
jmp end

smaller:
print "x is smaller"

end:
print "Done"
```

---

## Limitations

* No variable scoping (all variables are global)
* Minimal error handling
* Numbers are always floats
* Arrays store values only

---

##  License

See the `LICENSE` file for licensing details.
