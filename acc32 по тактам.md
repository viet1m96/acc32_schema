# Move Instructions:

## Load Immediate

**Syntax** : load_imm <value>
**Op** : acc <- value

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[PC] -> ACC
- PC = PC + 4

## Load

**Syntax** : load <offset>
**Op** : acc <- mem[pc + offset]

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[PC] -> DR
- PC = PC + 2

### Takt 3:

- DR(sign_extend) + PC -> Memory.addr
- M[addr] -> ACC

## Store:

**Syntax** : store <offset>
**Op**: mem[pc + offset] <- acc

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[PC] -> DR
- PC = PC + 2

### Takt 3:

- DR(sign_extend) + PC -> Memory.addr
- ACC -> M[addr]

## Load Address

**Syntax** : load_addr <address>
**Op** : acc <- mem[address]

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[PC] -> DR
- PC = PC + 4

### Takt 3:

- DR -> Memory.addr
- M[addr] -> ACC

## Store Address:

**Syntax** : store_addr <address>
**Op** : mem[address] <- acc

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[PC] -> DR
- PC = PC + 4

### Takt 3:

- DR -> Memory.addr
- ACC -> M[addr]

## Load Acc

**Syntax** : load_acc
**Op** : acc <- mem[acc]

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- ACC -> Memory.addr
- M[addr] -> ACC

## Store Indirect:

**Syntax** : store_ind <address>
**Op** : mem[mem[address]] <- acc

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[addr] -> DR
- PC = PC + 4

### Takt 3:

- DR -> Memory.addr
- M[addr] -> DR

### Takt 4:

- DR -> Memory.addr
- M[addr] -> ACC

---

# Arithmetic Instructions:

## Инстркуции (кроме **clv** и **сlc**) выполняются таким образом:

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[addr] -> DR
- PC = PC + 2

### Takt 3:

- ACC <operation> DR -> ACC

## Инстркуции **clv** и **сlc** выполняются таким образом:

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- 0 -> clc || 0 -> carry

---

# Bitwise Instructions:

## Инстркуции (кроме **not**) выполняются таким образом:

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- M[addr] -> DR
- PC = PC + 2

### Takt 3:

- ACC <operation> DR -> ACC

## Инстркуция **not** выполняется таким образом:

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- ACC = ~ACC

---

# Control Flow Instructions:

## Инструкции (кроме **jump**) выполняются таким образом:

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- comparison
- (mem_out -> PC) || (PC + 4 -> PC)

## Инструкция **jump** выполняется таким образом:

### Takt 1:

- PC -> Memory.addr
- M[PC] -> IR
- PC = PC + 1

### Takt 2:

- PC -> Memory.addr
- mem_out -> PC

---
