To open ipynb file go to colab.research.google.com and run cells
Examples of interpret work located in ipynb file or

"Lambda Calculus Interpreter

Enter lambda expressions using '\' or 'λ' for abstraction.

For example: (\x. x) y

Type 'quit' to exit.

λ> λx.x

Parsed term: (λx.x)

Reduced term: (λx.x)

λ> (λx.x) y

Parsed term: ((λx.x) y)

Reduced term: y

λ> (λx. x x) (λy. y)

Parsed term: ((λx.(x x)) (λy.y))

Reduced term: (λy.y)

λ> (λx. x) ((λy. y) z)

Parsed term: ((λx.x) ((λy.y) z))

Reduced term: z

λ> (λn. λf. λx. f (n f x)) λf. λx. f x

Parsed term: ((λn.(λf.(λx.(f ((n f) x))))) (λf.(λx.(f x))))

Reduced term: (λf.(λx.(f (f x))))

λ> (λp. λq. p q p) (λx. λy. x) (λx. λy. y)

Parsed term: (((λp.(λq.((p q) p))) (λx.(λy.x))) (λx.(λy.y)))

Reduced term: (λx.(λy.y))

λ> quit"

# Lambda Calculus Interpreter: Data Structures and Algorithms

---

## Data Structures

1. **Abstract Syntax Tree (AST)**
   - Lambda expressions are represented as tree nodes using three types of nodes:
     - `Variable` — a variable (`x`, `y`, etc.).
     - `Abstraction` — a lambda abstraction (`λx.M`).
     - `Application` — a function application (`(M N)`).

2. **Sets**
   - Used for storing **free variables** (`free_vars`), which helps during substitution.

3. **Lists**
   - Used in the **lexer** (for storing tokens) and **parser** (for parameters in multi-parameter abstractions).

---

## Algorithms

### 1. **Parsing**
   - Uses **recursive descent parsing**.
   - Main steps:
     1. The lexer converts the string into a list of tokens (`tokenize`).
     2. The parser constructs the AST based on rules:
        - `parse_abstraction` handles `λx.M`.
        - `parse_application` handles `(M N)`.
        - `parse_atom` handles variables and nested expressions.

### 2. **Substitution**
   - Used in **β-reduction** (`reduce_once`).
   - Implemented in the `substitute(var, value)` method:
     - If the variable matches `var`, it is replaced with `value`.
     - If the body has a variable with the same name (`param` in `Abstraction`), **α-conversion** (renaming) is performed.

### 3. **β-reduction (Beta Reduction)**
   - Uses a **recursive algorithm**:
     1. Checks if the expression is an **application** (`Application`).
     2. If the **function** is an abstraction (`λx.M`), replace `x` with the argument (`N`).
     3. If substitution is possible, perform it and continue recursion.
     4. If not, attempt to simplify the function or the argument.

### 4. **Alpha Conversion**
   - Prevents **variable capture**.
   - If there is a variable with the same name in the substituted expression, a **new unique name** is generated (`fresh_var`).
   - This prevents **unintended changes to the variable's value**.

### 5. **Term Reduction (Evaluation)**
   - `reduce_term(term, max_steps=1000)`
     - Performs reduction until no more β-reductions are possible or a step limit is reached.
     - Uses `reduce_once(term)` to perform one step at a time.
     - The algorithm is **greedy**, meaning it first tries to simplify the `func`, then the `arg`.

---

## Summary

This code uses:
- **Trees (AST)** to represent expressions.
- **Recursion** for term reduction.
- **Greedy algorithm** for β-reduction.
- **Alpha conversion** to avoid variable capture.


Example pf 
