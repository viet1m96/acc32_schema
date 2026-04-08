


# 1) Data movement

## `load_imm <address>` — 2 takt
 `ACC <- <address>`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `ACC <- M[PC]`
- `PC <- PC + 4`
---
## `load <offset>` — 3 takt
`ACC <- M[PC + offset]`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `ACC <- M[ALU_out]`
---

## `store <offset>` — 3 takt
 `M[PC + offset] <- ACC`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`

**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
- `BR <- ACC`

**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `M[ALU_out] <- BR`
---

## `load_addr <address>` — 3 takt
 `ACC <- M[address]`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 4`
**Takt 3**
- `Memory.addr <- DR`
- `ACC <- M[DR]`
---
## `store_addr <address>` — 3 takt
`M[address] <- ACC`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 4`
- `BR <- ACC`
**Takt 3**
- `Memory.addr <- DR`
- `M[DR] <- BR`
---
## `load_acc` — 2 takt
`ACC <- M[ACC]`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- ACC`
- `ACC <- M[ACC]`
---

## `store_ind <address>` — 4 takt
 `M[M[address]] <- ACC`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 4`
- `BR <- ACC`
**Takt 3**
- `Memory.addr <- DR`
- `DR <- M[DR]`
**Takt 4**
- `Memory.addr <- DR`
- `M[DR] <- BR`
---
# 2) Arithmetic
## `add <address>` — 4 takt
 `ACC <- ACC + M[PC + offset]`, update `C` and `V`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC + DR`
- `ACC <- ALU_out`
- `C <- carry`
- `V <- overflow`

---
## `sub <address>` — 4 takt
 `ACC <- ACC - M[PC + offset]`, update `V`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC - DR`
- `ACC <- ALU_out`
- `V <- overflow`
---
## `mul <address>` — 4 takt
 `ACC <- ACC * M[PC + offset]`, update `V`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC * DR`
- `ACC <- ALU_out`
- `V <- overflow`
---
## `div <address>` — 4 takt
`ACC <- ACC / M[PC + offset]`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC / DR`
- `ACC <- ALU_out`

---

## `rem <address>` — 4 takt
`ACC <- ACC % M[PC + offset]`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`

**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`

**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`

**Takt 4**
- `ALU <- ACC % DR`
- `ACC <- ALU_out`
---
## `clv` — 2 takt
 `V <- 0`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `clv = 1`
- `V <- 0`
---
## `clc` — 2 takt
 `C <- 0`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `clc = 1`
- `C <- 0`
---

# 3) Bitwise

## `shiftl <address>` — 4 takt
`ACC <- ACC << M[PC + offset]`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`

**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`

**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`

**Takt 4**
- `ALU <- ACC << DR`
- `ACC <- ALU_out`

---

## `shiftr <address>` — 4 takt
`ACC <- ACC >> M[PC + offset]`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC >> DR`
- `ACC <- ALU_out`
---
## `and <address>` — 4 takt
 `ACC <- ACC & M[PC + offset]`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC & DR`
- `ACC <- ALU_out`
---
## `or <address>` — 4 takt
`ACC <- ACC | M[PC + offset]`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC | DR`
- `ACC <- ALU_out`

---

## `xor <address>` — 4 takt
 `ACC <- ACC ^ M[PC + offset]`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `DR <- M[PC]`
- `PC <- PC + 2`
**Takt 3**
- `ALU <- PC + sign_extend(DR)`
- `Memory.addr <- ALU_out`
- `DR <- M[ALU_out]`
**Takt 4**
- `ALU <- ACC ^ DR`
- `ACC <- ALU_out`

---

## `not` — 2 takt
 `ACC <- ~ACC`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `ALU <- ~ACC`
- `ACC <- ALU_out`

---

# 4) Control flow
## `jmp <address>` — 2 takt
 `PC <- address`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- `PC <- M[PC]`
---

## `beqz <address>` — 2 takt
if `ACC == 0` -> `PC <- address`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`

**Takt 2**
- `Memory.addr <- PC`
- if `ACC == 0` -> `PC <- M[PC]`
- else if `ACC != 0` ->  `PC <- PC + 4`

---

## `bnez <address>` — 2 takt
 `ACC != 0` thì `PC <- address`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- if `ACC != 0` -> `PC <- M[PC]`
- if `ACC == 0` -> `PC <- PC + 4`

---

## `bgt <address>` — 2 takt
if `ACC > 0` -> `PC <- address`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- if `ACC > 0` -> `PC <- M[PC]`
- else `PC <- PC + 4`

---

## `ble <address>` — 2 takt
if `ACC < 0` -> `PC <- address`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- if `ACC < 0` -> `PC <- M[PC]`
- else `PC <- PC + 4`

---

## `bvs <address>` — 2 takt
if`V == 1` -> `PC <- address`
**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- if `V == 1` -> `PC <- M[PC]`
- else if `V == 0` -> `PC <- PC + 4`

---

## `bvc <address>` — 2 takt
if `V == 0` -> `PC <- address`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- if `V == 0` -> `PC <- M[PC]`
- else if `V == 1` -> `PC <- PC + 4`

---

## `bcs <address>` — 2 takt
if `C == 1` -> `PC <- address`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- if `C == 1` -> `PC <- M[PC]`
- if `C == 0` -> `PC <- PC + 4`

---

## `bcc <address>` — 2 takt
if `C == 0` -> `PC <- address`

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `Memory.addr <- PC`
- if `C == 0` -> `PC <- M[PC]`
- if `C == 1` -> `PC <- PC + 4`

---

## `halt` — 2 takt
stop CPU

**Takt 1**
- `Memory.addr <- PC`
- `IR <- M[PC]`
- `PC <- PC + 1`
**Takt 2**
- `HALT <- 1`

---
