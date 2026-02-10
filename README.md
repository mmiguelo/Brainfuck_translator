<a id="readme-top"></a>

<div align="center">

# 🧠 Brainfuck Translator

**A Brainfuck-to-C transpiler written in C — compile your brainf\*ck into real executables**

[![Language](https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c&logoColor=white)](https://en.wikipedia.org/wiki/C_(programming_language))
[![GitHub](https://img.shields.io/badge/Personal-Project-blueviolet?style=for-the-badge)](https://github.com/mmiguelo/Brainfuck_translator)

---

*Translate Brainfuck source files into equivalent C code, then compile and run them natively.*

</div>

---

## 📖 Table of Contents

- [About](#-about)
- [Brainfuck 101](#-brainfuck-101)
- [How the Translator Works](#%EF%B8%8F-how-the-translator-works)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#-usage)
- [Example](#-example)
- [Project Structure](#-project-structure)
- [Limitations](#-limitations)
- [Author](#-author)

---

## 🧠 About

This project is a **Brainfuck-to-C translator** — it reads a `.bf` source file and generates an equivalent `.c` file that, when compiled with any standard C compiler, produces a native executable with the same behavior as the original Brainfuck program.

Unlike an interpreter that executes Brainfuck at runtime, this **transpiler** approach gives you:

- ⚡ Native execution speed (compiled C)
- 🔍 Readable C output for debugging and learning
- 🔧 The ability to further modify the generated code

> This project was born from a personal initiative to uncover the secret message behind the 42 project **ft_printf** encrypted Brainfuck message.

---

## 📜 Brainfuck 101

Brainfuck operates on an array of memory cells (default 30,000 bytes, all initialized to zero) with a movable pointer. The entire language consists of just **8 instructions**:

<div align="center">

| Operator | C Equivalent | Description |
|:--------:|:-------------|:------------|
| `>` | `++ptr;` | Move pointer **right** one cell |
| `<` | `--ptr;` | Move pointer **left** one cell |
| `+` | `++*ptr;` | **Increment** the value at current cell |
| `-` | `--*ptr;` | **Decrement** the value at current cell |
| `.` | `putchar(*ptr);` | **Output** the character at current cell |
| `,` | `*ptr = getchar();` | **Input** a character into current cell |
| `[` | `while (*ptr) {` | **Loop start** — skip past `]` if current cell is 0 |
| `]` | `}` | **Loop end** — jump back to `[` if current cell is not 0 |

</div>

### Rules

- Any character **not listed above** is treated as a comment and ignored
- All memory cells are **initialized to zero**
- The pointer starts at the **leftmost** cell
- Loops can be **nested** — every `[` must have a matching `]`

---

## ⚙️ How the Translator Works

```
  input.bf                    output.c                  executable
 ┌───────────┐   bf2c        ┌──────────┐   gcc        ┌──────────┐
 │ Brainfuck │ ───────────▶  │  C code  │ ───────────▶ │  Binary  │
 │ source    │  translate    │  source  │  compile     │          │
 └───────────┘               └──────────┘              └──────────┘
```

The translator works in a single pass:

1. **Write C boilerplate** — `#include`, `main()`, array declaration, pointer setup
2. **Read the `.bf` file** line by line using `get_next_line`
3. **Map each operator** to its C equivalent via a switch statement
4. **Ignore** any non-operator characters (treated as comments)
5. **Write C closing** — trailing newline, `return 0`, closing brace

### Translation Example

```
Brainfuck:    +++[>++<-]>.

Becomes:
    #include <stdio.h>
    int main() {
        char array[3000000] = {0};
        char *ptr = array;
        ++*ptr;
        ++*ptr;
        ++*ptr;
        while (*ptr) {
            ++ptr;
            ++*ptr;
            ++*ptr;
            --ptr;
            --*ptr;
        }
        ++ptr;
        putchar(*ptr);
        printf("\n");
        return 0;
    }
```

---

## 🚀 Getting Started

### Prerequisites

- **GCC** or **CC** compiler
- **Make**

### Installation

```bash
# Clone the repository
git clone https://github.com/mmiguelo/Brainfuck_translator.git
cd Brainfuck_translator

# Build the translator
make
```

---

## 🎯 Usage

### Step 1 — Translate

```bash
./bf2c input.bf output.c
```

| Argument | Description |
|:---------|:------------|
| `input.bf` | Your Brainfuck source file (must exist) |
| `output.c` | The C file to generate (created automatically) |

### Step 2 — Compile the generated C

```bash
gcc output.c -o output
```

### Step 3 — Run

```bash
./output
```

### One-Liner

```bash
./bf2c input.bf output.c && gcc output.c -o output && ./output
```

---

## 💡 Example

**Brainfuck — Hello World** (`example.bf`):

```brainfuck
++++++++++[>+++++++>++++++++++>+++>+<<<<-]>++.>+.+++++++..+++.>++.
<<+++++++++++++++.>.+++.------.--------.>+.>.
```

**Run:**

```bash
./bf2c example.bf example.c
gcc example.c -o example
./example
```

**Output:**

```
Hello World!
```

---

## 📁 Project Structure

```
Brainfuck_translator/
├── 📄 Makefile              # Build system
├── 📖 README.md
├── bf2c.c                   # Translator: reads .bf, writes .c
├── test.bf                  # Sample Brainfuck program (42 ft_printf secret)
└── libft/                   # Custom C library (libft + ft_printf + ft_printf_fd + GNL)
```

---

## ⚠️ Limitations

| Limitation | Detail |
|:-----------|:-------|
| No syntax validation | Assumes valid Brainfuck — unmatched `[` / `]` won't be caught |
| No bounds checking | Memory overflows beyond the array are not detected |
| Static array size | Allocated at 3,000,000 bytes (adjustable in `write_opening()`) |
| Single-pass translation | No optimization (e.g., collapsing `+++` into `*ptr += 3`) |

---

## 🛠️ Makefile Targets

| Command | Description |
|:--------|:------------|
| `make` | Build the translator (`bf2c`) |
| `make clean` | Remove object files |
| `make fclean` | Remove object files and binary |
| `make re` | Full recompile |

---

## 👤 Author

**mmiguelo** — 42 Student

[![GitHub](https://img.shields.io/badge/GitHub-mmiguelo-181717?style=for-the-badge&logo=github)](https://github.com/mmiguelo)

---

<div align="center">

*Made with ❤️ at 42*

<p>(<a href="#readme-top">⬆️ back to top</a>)</p>

</div>
