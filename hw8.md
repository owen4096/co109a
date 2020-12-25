# 第九週

## mult.asm
<img src="https://github.com/owen4096/co109a/blob/master/04/mult/mult.png" width="500" height="400"  align=center /> 
<img src="https://github.com/owen4096/co109a/blob/master/04/mult/mult_hack.png" width="500" height="400"  align=center /> 
<pre><code>
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/04/Mult.asm

// Multiplies R0 and R1 and stores the result in R2.
// (R0, R1, R2 refer to RAM[0], RAM[1], and RAM[2], respectively.)

// Put your code here.
@2	
M=0	

@0
D=M
@END
D;JEQ	

@1
D=M
@END
D;JEQ	

@0	
D=M	
@3	
M=D	


(LOOP)

@1	
D=M	

@2	
M=D+M	

@3	
M=M-1	

D=M	
@LOOP	
D;JGT		


(END)
@END
0;JMP	

</code></pre>
