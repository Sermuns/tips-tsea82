# Instruktioner
Instruktioner är de grundläggande operationerna som en dator utför för att utföra en uppgift eller lösa en uppgift. AVR Assembler-instruktioner är specifika för AVR-processorer och används för att skriva program för dessa mikrokontroller.
- [Instruktioner](#instruktioner)
- [Förklaring av tabeller](#förklaring-av-tabeller)
  - [Statusregister (SREG)](#statusregister-sreg)
  - [Register och operand](#register-och-operand)
- [Instruktionsgrupper](#instruktionsgrupper)
  - [Arithmetic / Logic](#arithmetic--logic)
  - [Branch](#branch)
  - [Data transfer](#data-transfer)
  - [Bit and Bit-test](#bit-and-bit-test)
  - [MCU Control](#mcu-control)

# Förklaring av tabeller
## Statusregister (SREG)
|                       |                                                                                     |
| --------------------- | ----------------------------------------------------------------------------------- |
| SREG:                 | Statusregister                                                                      |
| C:                    | Carry-flagga                                                                        |
| Z:                    | Noll-flagga                                                                         |
| N:                    | Negativ flagga                                                                      |
| V:                    | Indikator för tvåkomplementsöverflöde                                               |
| S:                    | N ⊕ V, för teckentest                                                               |
| H:                    | Halvcarry-flagga                                                                    |
| T:                    | Överföringsbit som används av instruktionerna BLD och BST                           |
| I:                    | Global flagga för att aktivera/inaktivera avbrott (Global Interrupt Enable/Disable) |
## Register och operand
|                       |                                                                                     |
| --------------------- | ----------------------------------------------------------------------------------- |
| Rd:                   | Destinations- (och käll-)register i registerfilen                                   |
| Rr:                   | Källregister i registerfilen                                                        |
| R:                    | Resultatet efter att instruktionen har utförts                                      |
| K:                    | Konstant data                                                                       |
| k:                    | Konstant adress                                                                     |
| b:                    | Bit i registerfilen eller I/O-register (3-bit)                                      |
| s:                    | Bit i statusregistret (3-bit)                                                       |
| X,Y,Z:                | Indirekta adressregister (X=R27:R26, Y=R29:R28 och Z=R31:R30)                       |
| A:                    | Adress för I/O-platsen                                                              |
| q:                    | Förskjutning för direkt adressering (6-bit)                                         |

# Instruktionsgrupper
Nedan följer alla instruktioner som finns inom AVR-programmering, grupperade enligt liknande användningsområden.

## Arithmetic / Logic
| Mnemonics | Operands |          Description          |         Operation         |       Flags |
| :-------- | :------: | :---------------------------: | :-----------------------: | ----------: |
| ADD       |  Rd, Rr  |       Add without Carry       |       Rd ← Rd + Rr        | Z,C,N,V,S,H |
| ADC       |  Rd, Rr  |        Add with Carry         |     Rd ← Rd + Rr + C      | Z,C,N,V,S,H |
| ADIW      |  Rd, K   |     Add Immediate to Word     |   Rd+1:Rd ← Rd+1:Rd + K   |   Z,C,N,V,S |
| SUB       |  Rd, Rr  |    Subtract without Carry     |       Rd ← Rd - Rr        | Z,C,N,V,S,H |
| SUBI      |  Rd, K   |      Subtract Immediate       |        Rd ← Rd - K        | Z,C,N,V,S,H |
| SBC       |  Rd, Rr  |      Subtract with Carry      |     Rd ← Rd - Rr - C      | Z,C,N,V,S,H |
| SBCI      |  Rd, K   | Subtract Immediate with Carry |      Rd ← Rd - K - C      | Z,C,N,V,S,H |
| SBIW      |  Rd, K   | Subtract Immediate from Word  |   Rd+1:Rd ← Rd+1:Rd - K   |   Z,C,N,V,S |
| AND       |  Rd, Rr  |          Logical AND          |       Rd ← Rd • Rr        |     Z,N,V,S |
| ANDI      |  Rd, K   |  Logical AND with Immediate   |        Rd ← Rd • K        |     Z,N,V,S |
| OR        |  Rd, Rr  |          Logical OR           |       Rd ← Rd v Rr        |     Z,N,V,S |
| ORI       |  Rd, K   |   Logical OR with Immediate   |        Rd ← Rd v K        |     Z,N,V,S |
| EOR       |  Rd, Rr  |         Exclusive OR          |       Rd ← Rd ⊕ Rr        |     Z,N,V,S |
| COM       |    Rd    |       One’s Complement        |       Rd ← $FF - Rd       |   Z,C,N,V,S |
| NEG       |    Rd    |       Two’s Complement        |       Rd ← $00 - Rd       | Z,C,N,V,S,H |
| SBR       |   Rd,K   |    Set Bit(s) in Register     |        Rd ← Rd v K        |     Z,N,V,S |
| CBR       |   Rd,K   |   Clear Bit(s) in Register    |   Rd ← Rd • ($FFh - K)    |     Z,N,V,S |
| INC       |    Rd    |           Increment           |        Rd ← Rd + 1        |     Z,N,V,S |
| DEC       |    Rd    |           Decrement           |        Rd ← Rd - 1        |     Z,N,V,S |
| TST       |    Rd    |    Test for Zero or Minus     |       Rd ← Rd • Rd        |     Z,N,V,S |
| CLR       |    Rd    |        Clear Register         |       Rd ← Rd ⊕ Rd        |     Z,N,V,S |
| SER       |    Rd    |         Set Register          |         Rd ← $FF          |        None |
| MUL       |  Rd,Rr   |       Multiply Unsigned       |   R1:R0 ← Rd × Rr (UU)    |         Z,C |
| MULS      |  Rd,Rr   |        Multiply Signed        |   R1:R0 ← Rd × Rr (SS)    |         Z,C |
| MULSU     |  Rd,Rr   | Multiply Signed with Unsigned |   R1:R0 ← Rd × Rr (SU)    |         Z,C |
| FMUL      |  Rd,Rr   | Fractional Multiply Unsigned  | R1:R0 ← (Rd × Rr)<<1 (UU) |         Z,C |

## Branch
| Mnemonics |           Operands            |             Description             |             Operation              |        Flags |
| :-------- | :---------------------------: | :---------------------------------: | :--------------------------------: | -----------: |
| RJMP      |               k               |            Relative Jump            |          PC ← PC + k + 1           |         None |
| IJMP      |     Indirect Jump to (Z)      |     PC(15:0) ← Z, PC(21:16) ← 0     |                None                |        2 (1) |
| EIJMP     | Extended Indirect Jump to (Z) |   PC(15:0) ← Z, PC(21:16) ← EIND    |                None                |        2 (1) |
| JMP       |               k               |                Jump                 |               PC ← k               |         None |
| RCALL     |               k               |      Relative Call Subroutine       |          PC ← PC + k + 1           |         None |
| ICALL     |     Indirect Call to (Z)      |     PC(15:0) ← Z, PC(21:16) ← 0     |                None                | 3 / 4 (1)(4) |
| EICALL    | Extended Indirect Call to (Z) |   PC(15:0) ← Z, PC(21:16) ← EIND    |                None                |     4 (1)(4) |
| CALL      |               k               |           Call Subroutine           |               PC ← k               |         None |
| RET       |       Subroutine Return       |             PC ← STACK              |                None                |    4 / 5 (4) |
| RETI      |       Interrupt Return        |             PC ← STACK              |                 I                  |    4 / 5 (4) |
| CPSE      |             Rd,Rr             |       Compare, Skip if Equal        |   if (Rd = Rr) PC ← PC + 2 or 3    |         None |
| CP        |             Rd,Rr             |               Compare               |              Rd - Rr               |  Z,C,N,V,S,H |
| CPC       |             Rd,Rr             |         Compare with Carry          |            Rd - Rr - C             |  Z,C,N,V,S,H |
| CPI       |             Rd,K              |       Compare with Immediate        |               Rd - K               |  Z,C,N,V,S,H |
| SBRC      |             Rr, b             |   Skip if Bit in Register Cleared   |   if (Rr(b)=0) PC ← PC + 2 or 3    |         None |
| SBRS      |             Rr, b             |     Skip if Bit in Register Set     |   if (Rr(b)=1) PC ← PC + 2 or 3    |         None |
| SBIC      |             A, b              | Skip if Bit in I/O Register Cleared |  if(I/O(A,b)=0) PC ← PC + 2 or 3   |         None |
| SBIS      |             A, b              |   Skip if Bit in I/O Register Set   |  If(I/O(A,b)=1) PC ← PC + 2 or 3   |         None |
| BRBS      |             s, k              |      Branch if Status Flag Set      | if (SREG(s) = 1) then PC ←PC+k + 1 |         None |
| BRBC      |             s, k              |    Branch if Status Flag Cleared    | if (SREG(s) = 0) then PC ←PC+k + 1 |         None |
| BREQ      |               k               |           Branch if Equal           |  if (Z = 1) then PC ← PC + k + 1   |         None |
| BRNE      |               k               |         Branch if Not Equal         |  if (Z = 0) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRCS      |               k               |         Branch if Carry Set         |  if (C = 1) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRCC      |               k               |       Branch if Carry Cleared       |  if (C = 0) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRSH      |               k               |      Branch if Same or Higher       |  if (C = 0) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRLO      |               k               |           Branch if Lower           |  if (C = 1) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRMI      |               k               |           Branch if Minus           |  if (N = 1) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRPL      |               k               |           Branch if Plus            |  if (N = 0) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRGE      |               k               | Branch if Greater or Equal, Signed  | if (N ⊕ V= 0) then PC ← PC + k + 1 |         None | 1 / 2 |
| BRLT      |               k               |     Branch if Less Than, Signed     | if (N ⊕ V= 1) then PC ← PC + k + 1 |         None | 1 / 2 |
| BRHS      |               k               |    Branch if Half Carry Flag Set    |  if (H = 1) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRHC      |               k               |  Branch if Half Carry Flag Cleared  |  if (H = 0) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRTS      |               k               |        Branch if T Flag Set         |  if (T = 1) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRTC      |               k               |      Branch if T Flag Cleared       |  if (T = 0) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRVS      |               k               |   Branch if Overflow Flag is Set    |  if (V = 1) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRVC      |               k               | Branch if Overflow Flag is Cleared  |  if (V = 0) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRIE      |               k               |     Branch if Interrupt Enabled     |  if (I = 1) then PC ← PC + k + 1   |         None | 1 / 2 |
| BRID      |               k               |    Branch if Interrupt Disabled     |  if (I = 0) then PC ← PC + k + 1   |         None | 1 / 2 |

## Data transfer
| Mnemonics |    Operands    |                   Description                   |         Operation         | Flags |
| :-------- | :------------: | :---------------------------------------------: | :-----------------------: | ----: |
| MOV       |     Rd, Rr     |                  Copy Register                  |          Rd ← Rr          |  None |
| MOVW      |     Rd, Rr     |               Copy Register Pair                |     Rd+1:Rd ← Rr+1:Rr     |  None |
| LDI       |     Rd, K      |                 Load Immediate                  |          Rd ← K           |  None |
| LDS       |     Rd, k      |           Load Direct from data space           |         Rd ← (k)          |  None |
| LD        |     Rd, X      |                  Load Indirect                  |         Rd ← (X)          |  None |
| LD        |     Rd, X+     |        Load Indirect and Post-Increment         |    Rd ← (X), X ← X + 1    |  None |
| LD        |     Rd, -X     |         Load Indirect and Pre-Decrement         |    X ← X - 1, Rd ← (X)    |  None |
| LD        |     Rd, Y      |                  Load Indirect                  |         Rd ← (Y)          |  None |
| LD        |     Rd, Y+     |        Load Indirect and Post-Increment         |    Rd ← (Y), Y ← Y + 1    |  None |
| LD        |     Rd, -Y     |         Load Indirect and Pre-Decrement         |    Y ← Y - 1, Rd ← (Y)    |  None |
| LDD       |     Rd,Y+q     |         Load Indirect with Displacement         |       Rd ← (Y + q)        |  None |
| LD        |     Rd, Z      |                  Load Indirect                  |         Rd ← (Z)          |  None |
| LD        |     Rd, Z+     |        Load Indirect and Post-Increment         |     Rd ← (Z), Z ← Z+1     |  None |
| LD        |     Rd, -Z     |         Load Indirect and Pre-Decrement         |    Z ← Z - 1, Rd ← (Z)    |  None |
| LDD       |    Rd, Z+q     |         Load Indirect with Displacement         |       Rd ← (Z + q)        |  None |
| STS       |     k, Rr      |           Store Direct to data space            |         (k) ← Rd          |  None |
| ST        |     X, Rr      |                 Store Indirect                  |         (X) ← Rr          |  None |
| ST        |     X+, Rr     |        Store Indirect and Post-Increment        |    (X) ← Rr, X ← X + 1    |  None |
| ST        |     -X, Rr     |        Store Indirect and Pre-Decrement         |    X ← X - 1, (X) ← Rr    |  None |
| ST        |     Y, Rr      |                 Store Indirect                  |         (Y) ← Rr          |  None |
| ST        |     Y+, Rr     |        Store Indirect and Post-Increment        |    (Y) ← Rr, Y ← Y + 1    |  None |
| ST        |     -Y, Rr     |        Store Indirect and Pre-Decrement         |    Y ← Y - 1, (Y) ← Rr    |  None |
| STD       |    Y+q, Rr     |        Store Indirect with Displacement         |       (Y + q) ← Rr        |  None |
| ST        |     Z, Rr      |                 Store Indirect                  |         (Z) ← Rr          |  None |
| ST        |     Z+, Rr     |        Store Indirect and Post-Increment        |    (Z) ← Rr, Z ← Z + 1    |  None |
| ST        |     -Z, Rr     |        Store Indirect and Pre-Decrement         |    Z ← Z - 1, (Z) ← Rr    |  None |
| STD       |     Z+q,Rr     |        Store Indirect with Displacement         |       (Z + q) ← Rr        |  None |
| LPM       |    R0 ← (Z)    |               Load Program Memory               |                           |     3 |
| LPM       |     Rd, Z      |               Load Program Memory               |         Rd ← (Z)          |  None |
| LPM       |     Rd, Z+     |     Load Program Memory and Post-Increment      |    Rd ← (Z), Z ← Z + 1    |  None |
| ELPM      | R0 ← (RAMPZ:Z) |          Extended Load Program Memory           |                           |     3 |
| ELPM      |     Rd, Z      |          Extended Load Program Memory           |      Rd ← (RAMPZ:Z)       |  None |
| ELPM      |     Rd, Z+     | Extended Load Program Memory and Post-Increment | Rd ← (RAMPZ:Z), Z ← Z + 1 |  None |
| SPM       |                |              Store Program Memory               |        (Z) ← R1:R0        |  None |
| IN        |     Rd, A      |              In From I/O Location               |        Rd ← I/O(A)        |  None |
| OUT       |     A, Rr      |               Out To I/O Location               |        I/O(A) ← Rr        |  None |
| PUSH      |       Rr       |             Push Register on Stack              |        STACK ← Rr         |  None |
| POP       |       Rd       |             Pop Register from Stack             |        Rd ← STACK         |  None |

## Bit and Bit-test
| Mnemonic | Operands | Description                     | Flags Affected | Bytes   |
| -------- | -------- | ------------------------------- | -------------- | ------- |
| LSL      | Rd       | Logical Shift Left              | Z,C,N,V,H      | 1       |
| LSR      | Rd       | Logical Shift Right             | Z,C,N,V        | 1       |
| ROL      | Rd       | Rotate Left Through Carry       | Z,C,N,V,H      | 1       |
| ROR      | Rd       | Rotate Right Through Carry      | Z,C,N,V        | 1       |
| ASR      | Rd       | Arithmetic Shift Right          | Z,C,N,V        | 1       |
| SWAP     | Rd       | Swap Nibbles                    | None           | 1       |
| BSET     | s        | Flag Set                        | SREG(s) ← 1    | SREG(s) |
| BCLR     | s        | Flag Clear                      | SREG(s) ← 0    | SREG(s) |
| SBI      | A, b     | Set Bit in I/O Register         | I/O(A, b) ← 1  | None    |
| CBI      | A, b     | Clear Bit in I/O Register       | I/O(A, b) ← 0  | None    |
| BST      | Rr, b    | Bit Store from Register to T    | T ← Rr(b)      | T       |
| BLD      | Rd, b    | Bit Load from T to Register     | Rd(b) ← T      | None    |
| SEC      | -        | Set Carry                       | C ← 1          | C       |
| CLC      | -        | Clear Carry                     | C ← 0          | C       |
| SEN      | -        | Set Negative Flag               | N ← 1          | N       |
| CLN      | -        | Clear Negative Flag             | N ← 0          | N       |
| SEZ      | -        | Set Zero Flag                   | Z ← 1          | Z       |
| CLZ      | -        | Clear Zero Flag                 | Z ← 0          | Z       |
| SEI      | -        | Global Interrupt Enable         | I ← 1          | I       |
| CLI      | -        | Global Interrupt Disable        | I ← 0          | I       |
| SES      | -        | Set Signed Test Flag            | S ← 1          | S       |
| CLS      | -        | Clear Signed Test Flag          | S ← 0          | S       |
| SEV      | -        | Set Two’s Complement Overflow   | V ← 1          | V       |
| CLV      | -        | Clear Two’s Complement Overflow | V ← 0          | V       |
| SET      | -        | Set T in SREG                   | T ← 1          | T       |
| CLT      | -        | Clear T in SREG                 | T ← 0          | T       |
| SEH      | -        | Set Half Carry Flag in SREG     | H ← 1          | H       |
| CLH      | -        | Clear Half Carry Flag in SREG   | H ← 0          | H       |

## MCU Control
| Instruction | Description    | Operand | Cycles |
| ----------- | -------------- | ------- | ------ |
| BREAK       | Break          | None    | 1 (1)  |
| NOP         | No Operation   | None    | 1      |
| SLEEP       | Sleep          | None    | 1      |
| WDR         | Watchdog Reset | None    | 1      |