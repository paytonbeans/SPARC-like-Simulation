# SPARC-like-Simulation
C implementation of a spark like instruction set.

subset features
  32 32-bit registers, r0 always 0
  no register windows
  4-bit condition code, NZVC (negative, zero, overflow, carry)
  32-bit word addressing rather than byte addressing

simulator features
  command line file name for program in hex
  contents of memory echoed as they are read in
  final contents of registers and memory are printed on halt
  execution statistics are also printed on halt

subset of SPARC instruction formats

format 2:
                +--+-+---+---+------------------------+
branch is       |00|a|cnd|op2|..22-bit displacement...|
                +--+-+---+---+------------------------+
sethi is        |00|rdest|100|..22-bit displacement...|
                +--+-----+---+------------------------+
format 3:
                +--+-----+------+-----+-+--------+-----+
3-register is   |1x|rdest|.op3..|rsrc1|0|asi/fpop|rsrc2|
                +--+-----+------+-----+-+--------+-----+
immediate is    |1x|rdest|.op3..|rsrc1|1|signed 13-bit |
                +--+-----+------+-----+-+--------------+

ten SPARC-like instructions, with some having 3-register as well
as register-immediate forms

  aa..a is 22-bit signed displacement or sethi constant
  ddddd is 5-bit rdest register specifier
  sssss is 5-bit rsrc1 register specifier
  ttttt is 5-bit rsrc2 register specifier
  ii.ii is 13-bit signed immediate value

  00 _____ ___ aaaaaaaaaaaaaaaaaaaaaa   branch/sethi format
  00 01000 010                          ba, branch always
  00 01011 010                          bge, branch greater than or equal
  00 ddddd 100                          sethi    (also set)

  1_ ddddd ______ sssss 0 00000000 ttttt   reg-reg format
  1_ ddddd ______ sssss 1 iiiiiiiiiiiii    reg-imm format
  10       000000                          add   (also inc)
  10       000010                          or    (also set, clr)
  10       000100                          sub   (also dec)
  10       010100                          subcc (also cmp)
  10       100101                          sll
  11 ddddd 000000                          load
  11       000100                          store
