# ASCII Table Display in Assembly

This repository contains an assembly language project that displays the ASCII table on the screen. The program initializes the screen, iterates through the ASCII values, and prints each character along with its corresponding hexadecimal value.

## Features

- **Screen Initialization**: Sets up the screen for displaying the ASCII table.
- **ASCII Table Display**: Iterates through ASCII values (0-255) and prints each character with its hexadecimal representation.
- **Cursor Positioning**: Manages the position of the cursor to display characters in a formatted manner.

### Code Snippets

#### Screen Initialization
The program sets up the data segment and initializes variables for row and column positioning.

```assembly
.MODEL SMALL     ; Define memory model
.STACK 100h      ; Reserve stack space

.DATA            ; Data segment
FILA DB 0        ; Initialize row to 0
COLU DB 0        ; Initialize column to 0

.CODE            ; Code segment
START:
MOV AX, @DATA    ; Load data segment address
MOV DS, AX
```

#### ASCII Table Display Loop

The program uses a loop to iterate through ASCII values, convert them to hexadecimal, and print them along with the corresponding character.

```assembly
IMPCI MACRO CICLO
REPETIR:
    PUSH CX
    SHR CL, 4    ; Convert high nibble to ASCII
    ADD CL, 30H
    CMP CL, 39H
    JLE IMPRIMIR
    ADD CL, 7H

IMPRIMIR:
    IMPDIGITO CL ; Print high nibble

    POP CX
    AND CL, 0FH  ; Convert low nibble to ASCII
    ADD CL, 30H
    CMP CL, 39H
    JLE IMPRI
    ADD CL, 7H

IMPRI:
    IMPDIGITO CL ; Print low nibble
    CALL IGUAL   ; Print equal sign

    POP CX
    IMPRICARACTER CX ; Print ASCII character

    CURSOR FILA COLU ; Position cursor
    INC FILA
    CMP FILA, 24
    JNE CONTINUAR
    MOV FILA, 0
    ADD COLU, 7

CONTINUAR:
    POP CX
    INC CX
    CMP CX, 256
    JE FIN
    JMP REPETIR
IMPCI ENDM

```
#### Printing Macros

The program includes macros for printing digits, characters, and positioning the cursor.

```assembly
IMPDIGITO MACRO CL
    MOV AH, 02H
    MOV DL, CL
    INT 21H
IMPDIGITOO ENDM

IGUAL PROC NEAR
    MOV AH, 02H
    MOV DL, ':'
    INT 21H
    RET
IGUAL ENDP

IMPRICARACTER MACRO CX
    MOV AH, 02H
    MOV DL, CL
    INT 21H
IMPRICARACTER ENDM

CURSOR MACRO FILA COLU
    MOV AH, 02H
    MOV DH, FILA
    MOV DL, COLU
    INT 10H
CURSOR ENDM
```

### Usage

1.  Assemble and link the code using an assembler (e.g., TASM, MASM).
2.  Run the executable.
3.  The program will display the ASCII table on the screen, showing each character along with its hexadecimal value.

### Memory Variables

-   `FILA`: Stores the current row position for the cursor.
-   `COLU`: Stores the current column position for the cursor.