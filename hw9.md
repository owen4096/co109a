# 第九週

## fill.asm
<img src="https://github.com/owen4096/co109a/blob/master/04/fill/fill.png" width="800" height="600"  align=center /> 
<img src="https://github.com/owen4096/co109a/blob/master/04/fill/fill_hack.png" width="400" height="800"  align=center /> 
<pre><code>
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/04/Fill.asm

// Runs an infinite loop that listens to the keyboard input.
// When a key is pressed (any key), the program blackens the screen,
// i.e. writes "black" in every pixel;
// the screen should remain fully black as long as the key is pressed. 
// When no key is pressed, the program clears the screen, i.e. writes
// "white" in every pixel;
// the screen should remain fully clear as long as no key is pressed.

// Put your code here.
(RESTART)
@SCREEN
D=A
@0
M=D	//PUT SCREEN START LOCATION IN RAM0


(KBDCHECK)

@KBD
D=M
@BLACK
D;JGT	
@WHITE
D;JEQ	
@KBDCHECK
0;JMP

(BLACK)
@1
M=-1	
@CHANGE
0;JMP

(WHITE)
@1
M=0	
@CHANGE
0;JMP

(CHANGE)
@1	
D=M	

@0
A=M	
M=D	

@0
D=M+1	
@KBD
D=A-D	

@0
M=M+1	
A=M

@CHANGE
D;JGT	

@RESTART
0;JMP
</code></pre>
