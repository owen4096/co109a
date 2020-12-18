# 第五週

## ALU
<img src="https://github.com/owen4096/co109a/blob/master/02/ALU.png" width="500" height="400"  align=center /> 
<pre><code>
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/02/ALU.hdl

/**
 * The ALU (Arithmetic Logic Unit).
 * Computes one of the following functions:
 * x+y, x-y, y-x, 0, 1, -1, x, y, -x, -y, !x, !y,
 * x+1, y+1, x-1, y-1, x&y, x|y on two 16-bit inputs, 
 * according to 6 input bits denoted zx,nx,zy,ny,f,no.
 * In addition, the ALU computes two 1-bit outputs:
 * if the ALU output == 0, zr is set to 1; otherwise zr is set to 0;
 * if the ALU output < 0, ng is set to 1; otherwise ng is set to 0.
 */

// Implementation: the ALU logic manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) set x = 0        // 16-bit constant
// if (nx == 1) set x = !x       // bitwise not
// if (zy == 1) set y = 0        // 16-bit constant
// if (ny == 1) set y = !y       // bitwise not
// if (f == 1)  set out = x + y  // integer 2's complement addition
// if (f == 0)  set out = x & y  // bitwise and
// if (no == 1) set out = !out   // bitwise not
// if (out == 0) set zr = 1
// if (out < 0) set ng = 1

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
   // Put you code here:

   
    //ALU
    //level1(zx,zy)
    Mux16(a=x,b=false,sel=zx,out=x1);
    Mux16(a=y,b=false,sel=zy,out=y1);
    
    //level2(Not16,nx,ny)
    Not16(in=x1,out=nx1);
    Not16(in=y1,out=ny1);
    Mux16(a=x1,b=nx1,sel=nx,out=x2);
    Mux16(a=y1,b=ny1,sel=ny,out=y2);

    //level3(Add16,And16,f)
    Add16(a=x2,b=y2,out=x2ady2);
    And16(a=x2,b=y2,out=x2any2);
    Mux16(a=x2any2,b=x2ady2,sel=f,out=fout);

    //level4(Not16,no)
    Not16(in=fout,out=nfout);
    
    Mux16(a=fout,b=nfout,sel=no,out=out,out[0..7]=Highway,out[8..15]=Lowway,out[15]=ng);
    Or8Way(in=Highway,out=o81);
    Or8Way(in=Lowway,out=o82);
    Or(a=o81,b=o82,out=o1);
    Not(in=o1,out=zr);
}
</code></pre>

## ALU-nostaat
<img src="https://github.com/owen4096/co109a/blob/master/02/ALU-nostat.png" width="500" height="400"  align=center /> 
<pre><code>
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/02/ALU.hdl

/**
 * The ALU (Arithmetic Logic Unit).
 * Computes one of the following functions:
 * x+y, x-y, y-x, 0, 1, -1, x, y, -x, -y, !x, !y,
 * x+1, y+1, x-1, y-1, x&y, x|y on two 16-bit inputs, 
 * according to 6 input bits denoted zx,nx,zy,ny,f,no.
 * In addition, the ALU computes two 1-bit outputs:
 * if the ALU output == 0, zr is set to 1; otherwise zr is set to 0;
 * if the ALU output < 0, ng is set to 1; otherwise ng is set to 0.
 */

// Implementation: the ALU logic manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) set x = 0        // 16-bit constant
// if (nx == 1) set x = !x       // bitwise not
// if (zy == 1) set y = 0        // 16-bit constant
// if (ny == 1) set y = !y       // bitwise not
// if (f == 1)  set out = x + y  // integer 2's complement addition
// if (f == 0)  set out = x & y  // bitwise and
// if (no == 1) set out = !out   // bitwise not
// if (out == 0) set zr = 1
// if (out < 0) set ng = 1

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
    // Put you code here:

    //ALU-nostat(no zr && ng)
    //level1(zx,zy)
    Mux16(a=x,b=false,sel=zx,out=x1);
    Mux16(a=y,b=false,sel=zy,out=y1);
    
    //level2(Not16,nx,ny)
    Not16(in=x1,out=nx1);
    Not16(in=y1,out=ny1);
    Mux16(a=x1,b=nx1,sel=nx,out=x2);
    Mux16(a=y1,b=ny1,sel=ny,out=y2);

    //level3(Add16,And16,f)
    Add16(a=x2,b=y2,out=x2ady2);
    And16(a=x2,b=y2,out=x2any2);
    Mux16(a=x2any2,b=x2ady2,sel=f,out=fout);

    //level4(Not16,no)
    Not16(in=fout,out=nfout);
    Mux16(a=fout,b=nfout,sel=no,out=out);
}
</code></pre>