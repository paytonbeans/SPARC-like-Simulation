
# SPARC-like-Simulation
C implementation of a spark like instruction set.

**subset features**
- 32 32-bit registers, r0 always 0
- no register windows
- 4-bit condition code, NZVC (negative, zero, overflow, carry)
- 32-bit word addressing rather than byte addressing<br>

**simulator features**
- command line file name for program in hex
- contents of memory echoed as they are read in
- final contents of registers and memory are printed on halt
- execution statistics are also printed on halt<br>

**subset of SPARC instruction formats**<br>
format 2:           
branch and sethi<br> 
+--+-+---+---+------------------------+<br> 
|00|a|cnd|op2|..22-bit displacement...|<br> 
+--+-+---+---+------------------------+<br> 
|00|rdest|100|..22-bit displacement...|<br> 
+--+-----+---+------------------------+<br>
format 3: <br>
3-register and immediate<br> 
+--+-----+------+-----+-+--------+-----+<br> 
|1x|rdest|.op3..|rsrc1|0|asi/fpop|rsrc2|<br> 
+--+-----+------+-----+-+--------+-----+<br> 
|1x|rdest|.op3..|rsrc1|1|signed 13-bit |<br> 
+--+-----+------+-----+-+--------------+<br>

ten SPARC-like instructions, with some having 3-register as well
as register-immediate forms<br>

- aa..a is 22-bit signed displacement or sethi constant<br>
- ddddd is 5-bit rdest register specifier
- sssss is 5-bit rsrc1 register specifier
- ttttt is 5-bit rsrc2 register specifier
- ii.ii is 13-bit signed immediate value<br>

  00 _____ ___ aaaaaaaaaaaaaaaaaaaaaa ... branch/sethi format<br>
  00 01000 010 ... ... ... ... ... ... ... ... ... ... ... .. . ba, branch always<br>
  00 01011 010 ... ... ... ... ... ... ... ... ... ... ... .. . bge, branch greater than or equal<br>
  00 ddddd 100 ... ... ... ... ... ... ... ... ... ... ... ... sethi    (also set)<br>
  
  1_ ddddd ______ sssss 0 00000000 ttttt   ... reg-reg format<br>
  1_ ddddd ______ sssss 1 iiiiiiiiiiiii ... ... ... .. . reg-imm format<br>
  10 ddddd 000000 ... ... ... ... ... ... ... ... ... .. . add   (also inc)<br>
  10 ddddd 000010 ... ... ... ... ... ... ... ... ... .. . or    (also set, clr)<br>
  10 ddddd 000100 ... ... ... ... ... ... ... ... ... .. . sub   (also dec)<br>
  10 ddddd 010100 ... ... ... ... ... ... ... ... ... .. . subcc (also cmp)<br>
  10 ddddd 100101 ... ... ... ... ... ... ... ... ... .. . sll<br>
  11 ddddd 000000 ... ... ... ... ... ... ... ... ... .. . load<br>
  11 ddddd  000100 ... ... ... ... ... ... ... ... ... .. . store<br>
