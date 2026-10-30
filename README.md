# xmlasm
Concept (eso) language inspired by XML format

# (CONCEPT) PROJECT XML-ASM (Esolang Specification & Blueprint)

### Concept Architecture

**Structural Hierarchy:**  
XML controls scoping, memory allocation (`<table>`), labels, and macros;  
embedded `<opcode>` blocks execute low-level assembly operations.  

**Argument Substitution:**  
Macros use `%N` placeholders for parameter injection at parse time.  

**Scope Rules:**  
Nested `<stack>` blocks inherit parent variable tables but maintain isolated memory frames for operands.  

Mock-up of a program:  
```xml
<!-- experimental -->

<program name="xml-asm-blueprint" version="0.1">

    <!-- 1. DATA TABLE & SCOPE DECLARATION -->
    <!-- Variables allocated with explicit data types and default values -->
    <table>
        <symbol ref="$limit" type="int16">8</symbol>
        <symbol ref="$counter" type="int16">0</symbol>
    </table>

    <!-- 2. REUSABLE MACROS (High-Level Abstraction) -->
    <!-- Inline routines expand directly into the target execution stack -->
    <macro id="function_foo">
        <!-- Expects variable reference to be supplied via attribute -->
        <opcode>
            PUSH, %0      <!-- %0 represents the passed argument ($counter) -->
            PUSH, #1
            ADD
            POP, %0
        </opcode>
    </macro>

    <!-- 3. MAIN EXECUTION STACK -->
    <!-- Scoped memory frame with explicit depth tracking -->
    <stack name="main_scope" size="4">

        <label id="loop_start"/>

        <!-- Core Logic Block -->
        <opcode target="main_scope">
            PUSH, $limit
            PUSH, $counter
            JEQ, [loop_end]
        </opcode>

        <!-- Macro Invocation -->
        <call macro="function_foo" arg="$counter"/>

        <opcode target="main_scope">
            JMP, [loop_start]
        </opcode>

        <label id="loop_end"/>

        <!-- 4. SYSTEM CALL / OUTPUT -->
        <!-- Channel 1: Console | Function 2: Write | Payload: $counter -->
        <opcode target="main_scope">
            PUSH, #1
            PUSH, #2
            PUSH, $counter
            SCALL
        </opcode>

    </stack>

</program>

```
