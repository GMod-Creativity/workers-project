Update ram_ycpu.txt and expression_compiler.txt
The code of WMBasic (WireMod Basic, not really original) is located in data/e2files/test.txt of garrysmod folder.

The program is made of functions (arguments and "return" opcode does not work for now !) :

function func_name(arg1, arg2, arg3)
{

}

Variables defined in functions will be local to that function, e.g. it will not interfere with other function's variables.
There is a special function, called by default "main", wich you must define because the program will start here.

You can define global variables tough, with the "global" opcode like this :

global variable1;
global variable2;
etc...

You must define globals before any function definition, and outside any block.
There are 8 special global variables : port1 to port8, these are globals and are corresponding to their output/input port.

Functions' block contains statements : either an expression assignement like

pi = 3.14;
r = 2;
areaOfCircle = pi * r * r;
surface_of_circle = pi * r * 2;

(expression can't contain function calls for now, meaning you can't do "time = curtime();" )

Or a function call (as I said before, arguments are pushed but not assigned for now) :

#################################################

global globalNumber;

function square()
{
    globalNumber = globalNumber * globalNumber;
}

function main()
{
    globalNumber = 4;
    square();
    port1 = globalNumber;
}

#################################################

(port1 will be outputted to 16).

You must define a function before using it elsewhere.

Example code (test.txt) :

#################################################

global number;

function sum(n1, n2)
{
    n1 = (3 + 2) * 5; n2 = 5 + (5 + 2) * 10;
    number = n1 + n2;
}
    
function main()
{
    sum(5, 5);
    port1 = number;
}

#################################################

This code will call sum(), and sum will set number (defined as global so main() can get access to it) to n1 + n2, wich for now are only defined into sum() function body.
This will output 100 to port 1.

Wiring :
Wire "RAM" inputs of the two E2s to a 1024*1024 ram.
Wire "Mem" zCPU's input to another 1024*1024 ram.
Wire zCPU's "Reset" to the E2's one.
Wire a screen to the port1, and type "/load" in chat.

If you want smaller RAM, make sure the line 20 of yCPU is changed to "Mem[0] = RamSize ^ 2 - 1" where RamSize is a value between 32 and 1024.