# Human Based Paper Computer (HBPC-1)

## Introduction

My child wanted to understand how computers work.  
That's why I did this.  
Sorry, I'm not a specialist in CPU developing. So this is just a simplification.  

## Assembly

Print memory.pdf. Cut apart horizontal lines and glue them together. You need to glue as much lines as you need for your program. It's the memory of your computer.  
Make a construction like on the image below.
I used Lego, but you can use anything you want.
Self test:  
![alt Human Based Paper Computer - 1: Self Test](https://raw.githubusercontent.com/DKurilo/hbpc/master/HBPC-1-self-test.jpg)
Ready to work:  
![alt Human Based Paper Computer - 1: Ready to work](https://raw.githubusercontent.com/DKurilo/hbpc/master/HBPC-1-ready-to-work.jpg)
The bottom panel is your CPU. It contains working register and 3 flags: overflow flag, negative flag and equal flag. Very simple.  
The top panel is I/O module. We have very simple I/O. It contains 7 8-segment indicators and 8 buttons.  
Also it's better to use a pencil and eraser, not a pen. So you will be able to rewrite memory.  

## How it works

You need to write a program in your computer memory. Use a pen, write an instruction, if the instruction requires data, the next cell is for the data.  
Surely, you can code instructions and use numbers for them. And if you want to show how stack overflow or any other simple vulnerabilities work you need to write code near the instruction, but it's easier for a child to use instruction names instead of code.
Place the memory in your computer. First cell is in the working window.  
And "turn on"!
Empty cell is "nop" instruction, so you can just go to the next instruction.  
Read the instruction in working window. If it's a single instruction (like nop) do what you need to do as you are a CPU. If it's one or more operand instruction, move to the next block and read data. Then perform the action you can use a separate piece of paper to make calculations.  
If you want to use two or more operand instructions, it's possible that you need to upgrade your CPU and use more inner registers.  

## Instruction set

We have a human based main Arithmetic Logic unit. So you can use any instruction set you want.  
I tried to make some basic instruction set. Here are our opcodes:  

### Instruction Set Nomenclature

R - CPU register.  
Rn - Memory register, n can be any natural number from 0 to 255.  
K - constant ( 0 to 255 )  
V - Overflow Flag  
N - Negative Flag  
E - Equal Flag  

### Memory

R - CPU working register (8 bits).  
R00 - Input Device. 8 buttons.  
R01 - R07 - Output devices. 7 8-segment indicators.  
R08 - R255 - common purpose memory.  

### Instructions
nop - (00000000) - no operation. Just go to the next step.  
ld Rn - (00000001) - load Rn content to R. Set R on the CPU panel.  
mov Rn - (00000010) - move the content of R to Rn. Find the cell, erase its current value and write the new one.  
setV - (00000011) - set flag V to 1. Do it on the CPU panel.  
setN - (00000100) - set flag N to 1. Do it on the CPU panel.  
setE - (00000101) - set flag E to 1. Do it on the CPU panel.  
clearV - (00000110) - clear flag V. Do it on the CPU panel.  
clearN - (00000111) - clear flag N. Do it on the CPU panel.  
clearE - (00001000) - clear flag E. Do it on the CPU panel.  
setR K - (00001001) - copy the constant to R. Read the next cell and set R on the CPU panel.  
cp Rn - (00001010) - compare R and Rn, If R is equal to Rn it sets flag E and clear flag N, if R is not equal to Rn it clears flag E and in this case if R is greater than Rn it clears flag N, if R is less than Rn it sets flag N). You need to find Rn, compare it with R and change flags E and N on the CPU panel. Do it carefully!  
jmp - (00001011) - jump to register with adress in R. You need to move your memory to the proper place.  
jmpG - (00001100) - If flag E is 0 and N is 1, jump to register with adress in R. You need to move your memory to the proper place. Otherwise just skip.  
jmpL - (00001101) - If flag E is 0 and N is 0, jump to register with adress in R. You need to move your memory to the proper place. Otherwise just skip.  
jmpE - (00001110) - If flag E is 1, jump to register with adress in R. You need to move your memory to the proper place. Otherwise just skip.  
jmpNE - (00001111) - If flag E is 0, jump to register with adress in R. You need to move your memory to the proper place. Otherwise just skip.  
jmpV - (00010000) - If flag V is 1 (overflow), jump to register with adress in R. You need to move your memory to the proper place. Otherwise just skip.  
jmpNV - (00010001) - If flag V is 0 (not overflow), jump to register with adress in R. You need to move your memory to the proper place. Otherwise just skip.  
sum Rn - (00010010) - arithmetic summation R = R + Rn. In case of overflow you need to set V flag. Find cell Rn and add it to R.   
sub Rn - (00010011) - arithmetic substraction R = R - Rn. In case Rn is greater than R set N flag. Find cell Rn and substract it from R.  
and Rn - (00010100) - logic 'and'. R = R and Rn. Find cell Rn and perform the operation on the CPU panel.  
or Rn - (00010101) - logic 'or'. R = R or Rn. Find cell Rn and perform the operation on the CPU panel.  
xor Rn - (00010111) - logic 'xor'. R = R xor Rn. Find cell Rn and perform the operation on the CPU panel.  
ls - (00011000) - left shift. Shift R to the left (it's equal to multiply by 2 for natural numbers). In case R has 1 in the high order bit, you need to set flag V (overflow).  
rs - (00011001) - right shift. Shift R to the right (it's equal to division by 2 with possible data loss for natural numbers).  


### Program examples

1. Running light

```
00001000 setR 00000001  # we start from the first segment  
00001010 mov 00000001   # the first segment is lit  
00001100 ld 00000001    # load current segments  
00001110 cp 00100011    # if the last segment is on  
00010000 setR 00001000  # we start a new cycle  
00010010 jmpE  
00010011 ld 00000001    # load current segments  
00010101 ls             # move a segment  
00010110 mov 00000001   # and send it to output  
00011000 ld 00000000    # load buttons  
00011010 cp 00100100    # compare to our mask (you can change the program to check for the exact button, think how to do it)  
00011100 setR 00100101  # if the button is pressed, end the program  
00011110 jmpNE          # in case any button is pressed end the program  
00011111 setR 00001100  # else go to "light the next segment" part  
00100001 jmp  
00100010 nop            # just nop  
00100011 10000000       # constant to find if the last segment is lit  
00100100 00000000       # mask for the button  
00100101 nop  
```
