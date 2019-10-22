![Iti Machine](https://i.imgur.com/x1zaq4u.png "logo")

# Overview

Create by: Felipe Resende (feliperesendehq@outlook.com)

__Iti Machine__ is a project of fantsy console like a virtual machine. Iti uses its own architecture and programming language, KIS. Different from a modern machine, this virtual machine is like old console video games dataset and doesn't have a lot of resourses, like garbage collector or memory manager. Then you have the power about the Iti Machine and you can use like you like.

__PC Windows Specs:__
Fantasy CPU: 32-bit simplified version of RISC 16.
RAM Memory : 500kb (128k array of interger)
ROM File: Max of 512mb.
Resolution : 128 x 128 ARGB32 sprite.
Audio: In development...
Device Support: Keyborad for while.
Emulator : Windows 32 or 64 bits.

# Files and Emulation
The emulator and internal builder runs in __Unity 2018.4.5f1__ and when you open *ItiMachine.exe*, it will read the script file in Project folder, builder its and generate the ROM and then, will fetch the ROM file in the virtual machine.

#### File Struct:
+ ItiMachine.exe
+ UnityPlayer.dll
+ Project
    + Scripts
		+ main.kis
		+ var.kis
    + Sprites
    	+ 0.png
+ MonoBleedingEdge
+ ItiMachine_Data

__ItiMachine.exe__ executable of the game project
__main.kis__ is the code of your game. It uses KIS language.
__main.var__ is the file to you write the variables.
__0.png__ is the spritesheet of your game. You can change the lenght, but is recommended maintain multiples of 2 like 256 x 256 or 1024 x 1024 pixels. The format must be PNG. 

# KIS Language
The purpose of KIS Language is be a simple way to programming hardwares with very low memomry and CPU usage. KIS sintax is similar to ruby or python and it works like a command line. The main feature is about the variables, they are declared in another file (__main.var__) and the logic code is type in __main.kis__. The order of variables is literaly the order of RAM memory registers and the amount of variables is the amount of memory statically allocated. You don't need allocate and free memory because you don't have memory manager. You aways can use all the memory you address. There is only signed interger type in Iti Machine with 32-bits. The value is from -2.147.483.648‬ to 2.147.483.648‬. 

The language is blockly and the spaces between the operators are important. The statments are defined by line breaks and ou can identity your code. You can only make this actions:

__Variables__ are defined in a differt file named *main.var*. To create variables, only write a name and break the line. Your variables cannot start with a number in first char and can't have space, but you can use "_" or "." to organize your code. Look the example code below:

```javascript
true
false
x
y
direction
password
life
```
The order of declarion is the same order of allocation in memory. For example: if you create a pointer to *y* and increases the poiter +1, it will pointer to *direction* variable.

__Set expressions__ is when you uses the *=* char to define the value of a variable with a math expretion that can uses numbers, operators and variables.
```javascript
helloWorld = 64
true = 1
false = 0
x = a + b
y = 120 + a / 23 << 3
```
There aren't parentesis in this language and the expression are readed in the order of operators from left to right. Remember, the spaces between operators are important:
```javascript
//Don't make this
my_var = a+b					// a variable are touching in b.
x= a * 4 								// x variable are touching in = operator.
time = (a + b)*deltatime	// There aren't parentesis.
a = b + x = d						// = oprator is used only to set a variable to expression.
```
__If__ statment uses to expressions and make a logic operation between the results. If the result is true, the code inside the block will run. In the end, close the block with *endif*. You can uses only one logic operator by *if* statement.
```javascript
if x + area > 16 * powerup
	x = 16 - shift
    video.draw16 : x y sprite diretion
endif
```
There isn't else or others way to make ifs statements. If you want to make a else, declare other if with inverse logic.
```python
if x + area <= 16 * powerup
	x = x - 1
	if x < 128 
		x = 128
	endif
	sprite = down
    video.draw16 : x y sprite diretion
endif
```
__While__ statments are used to create loops. Use a logic operator between two expressions and while the logic isn't false, the logic inside the block code will repeat. Remenber, if the logic never ends, __the Iti Machine will crash__.
```javascript
i = 0
while i < 32
	video.draw32 : xs ys anchorSprite 1
	i = i + 1
endwhile

//This code draw tiles in the screen
x = 0
while x < 128
	y = 0
	while y < 128
		video.draw32 : x y tile 1
		y = y + 32
	endwhile
	x = x + 32
endwhile
```
__Functions__ are create with __fun__ keyword. The parameters of functions are separted with spaces and you can't write expressions in a fun declarion. In the end, use endfun to finish a function. To call the functions, only write its name and throw the paremeters after *:* char.
```javascript
fun DrawSword : spriteId 
	spriteId = spriteId + animationFrame
	video.draw16 : x_hand y_hand spriteId right
endfun

if powerUp > 3
	DrawSword : spriteSword1
endif
if powerUp <= 2
	DrawSword : spriteSword2
endif
```
The functions works like routines, so they don't return values. You can't set a expression with a function. The parameters of functions are a copy of variables used in functions.

__Pointers__ are modifyers of values to control the memory acess. You can use *#* to get the addres of variable and *@* to read a memory position.
```javascript
// main.var file:
//enemyListLength stores the lenght and it is the index of list
enemyListLength
enemyPosition0
enemyPosition1
enemyPosition2
enemyPosition3
enemyPosition4
enemyPosition5

//main.kis file:
//This will make the enemys move 8 pixels to right
enemyListLength = 6
i = #enemyListLength + 1
while i < #enemyListLength + enemyListLength
	@i = @i + 8
		video.draw8 : enemyPosition0 y enemySprite 1
	i = i + 1
endwhile
```
You can use *@* to read a direct memory position with a magic number. *#* isn't aplicable to numbers, only to variables.
```javascript
readMemory = @8843
```
This feature allow you reuse functions to different purpose. If you want send the real variable in a function, you can send the address.
```javascript
fun Damage : entityLife value 
	@entityLife = @entityLife - value
endfun

Damage : #palyerLife
Damage : #enemy1Life
Damage : #enemy2Life
```
__Video functions__ are used to draw sprites on the screen. Iti machine doesn't have states and each frame, if you don't call the sprite function, it will disappear. The tileset is like a grid of tiles 8x8. Check the sample below:

![sample](https://i.imgur.com/jPMlBNs.png "sample")	![Grid](https://i.imgur.com/56C2klm.png "Grid")	![Grid on Sample](https://i.imgur.com/jhsXNbZ.png "Grid on Sample")

When you use the *video.draw8 :* function, the third argument is the id on tileset. If you want to draw the pink square on center of screen, you can write this way:

```javascript
//video.draw8 : x y id dir
//This will draw a pink square sprite on the screen in x and y position.
// id is the sprite location in the spritesheet and dir defines the horizontal flip.
// if dir is -1, the sprite will flip in horizontal.
video.draw8 : 56 56 22 1
```
If you want to draw a greater sprite with 16x16 or 32x32 pixels you can use a variation of the funcion:
```javascript
//video.draw16 : x y id dir
//This will draw a girl sprite on the screen in x and y position.
video.draw16 : 52 52 32 1
```
The lenght of sprite don't change the patter of spritesheet grid, in other hand, this means the function cuts the spritesheet on id position and the number in the end of function determines the lenght of sprite. The Id grid can grow up with the lenght of tileset, but the patter is the same.

__Inputs__ is a module to read a keyboard. Only read the values to get if the key was pressed. The horizontal or and vertical axis are defined like 1, 0 or -1. There are 2 axis and 4 keys to read in the device driver.
```
if input.horizontal > 0
	MoveToRight : speed
endif

if input.vertical < 0 
	ReduceVelocity : playerVelocity
endif

if input.fire == 1
	Shoot : direction, bulletId
endif

if input.water == 0
	Down : gravity downSpeed
endif
```
__Table of inputs and keyboard reference:__

| Input Name  | Key |
| ------------- | ------------- |
| input.horizontal  | A or D Keys |
| input.vertical | W or S Keys |
| input.fire  | Return Key |
| input.water | Space Key |
| input.earth  | E key |
| input.air | F key |

#Iti Machine Architecture
The Iti Machine cpu has 26 instructions with 32-bit register. The first 5 bits from left define the instruction to run and the others 27 are used to data. The machine has 1024 register to stack and 7 special registers:

__PC__: Defines the current instruction to read.
__stackCount__: Defines the lenght of stack.
$0 __worka__: First argument in a matematic operation.
$1 __workb__: Second argument in a matematic operation.
$2 __compa__: First argument in a logic operation.
$3 __compb__: Second argument in a logic operation.
$4 __target__: Used to stores temporarily values that will used in operations or to transport values between registers.

For while, the register $7 and $8 are used by video driver to stores the render instructions. You can check the assembly in the table below:

| ID | OpCode  | Name | Description | 
| ------------- | ------------- | ------------- | ------------- |
| 0 | 00000 | SET | *target* recives __data__ value |
|1|00001 | MOVE IN | *target* recives the value of __data__ address|
|2|00010 | MOVE OUT | Address memory defined by __data__ recives the value in*target*  address|
|3|00011 | GOTO | *PC* = __data__ |
|4|00100 | JUMP | *PC*  = value of memory defined by __data__ |
|5|00101 | ENQUEE | The top of stack recives the value of __data__. *StackCount*  +1.|
|6|00110 | DEQUEE |  *StackCount*  -1. Address memory defined by __data__ recives the value from top of stack|
|7|00111 | SUM | Address memory defined by __data__ recives the sum of *worka* and *workb* |
|8|01000 | SUB | Address memory defined by __data__ recives the subtraction of *worka* and *workb*  |
|9|01001 | MUL | Address memory defined by __data__ recives the multiplication of *worka* and *workb*  |
|10|01010 | DIV | Address memory defined by __data__ recives the division of *worka* and *workb*   |
|11|01010 | MOD | Address memory defined by __data__ recives the module of *worka* and *workb*   |
|12|01011 | SHIFT LEFT | Shift to left the bits of Address memory defined by __data__  |
|13|01100 | SHIFT RIGHT | Shift to right the bits of Address memory defined by __data__   |
|14|01101 | OR | Address memory defined by __data__ recives the *or* operation of *worka* and *workb*  |
|15|01110 | AND | Address memory defined by __data__ recives the *and* operation of *worka* and *workb*  |
|16|01111 | NOT | Invert the bits of address memory defined by __data__ |
|17|10000 | EQUAL LESS |  If *compa* is equal or less than *compb*, the value of *PC* = __data__ |
|18|10001 | EQUAL GREATER | If *compa*  euqal or greater than *compb*, the value of *PC* = __data__ |
|19|10010 | LESS | If *compa* is less than *compb*, the value of *PC* = __data__ |
|20|10011 | GREATER | If *compa* is greater than *compb*, the value of *PC* = __data__|
|21|10100 | EQUAL | If *compa* is equal *compb*, the value of *PC* = __data__|
|22|10101 | DIFF | If *compa* different *compb*, the value of *PC* = __data__|
|23|10110 | PASS | Pass this instruction|
|24|10111 | POINT | The address of memory defined by *target* recives the value of memory of address defined by the value of memory defined by  __data__ |
|25|11000 | POINT OUT | The address of memory defined by the value  of memory defined by __data__ recives the value of memory defined by *target*|

Check below a example of assembly convertion.

```
a = 123			//$22 memory address
b = 44			//$33 memory address
c = a + b		//$44 memory adrress


//Assembly convertion:
SET		: 123		//target recives the 123 immediate
MOVE OUT 	: $22		//$22 address recives the value of target
SET 		: 44 		//target recives the 44 immediate
MOVE OUT 	: $33 		//The $33 recives the value from target

MOVE IN	 	: $22		//target recives the value from $44
MOVE OUT 	: $0		//worka recives the value of target
MOVE IN		: $33 		//target recives the value from $22
MOVE OUT 	: $1		//workb recives the value from target
SUM		: $44		//$44 recives the sum of worka and workb

```

# Quick Start

+ Look and download the Iti Machine
+ Goto Project > Scripts*
+ Open *main.kis*  and *main.var* files with any text editor
+ Edit or write your code
+ Save the file and run *ItiMachine.exe*

See the default code description:
```
//Defines some constants 
true = 1
false = 0
left = -1
right = 1
idle = 48

//Draw the grass sprite in the background, the id of grass sprite is 54 and it has 16x16 pixels.

tile = 54
tiles_x = 0
while tiles_x < 128
	tiles_y = 0
	while tiles_y < 128
        video.draw16 : tiles_x tiles_y tile right
		tiles_y = tiles_y + 16
	endwhile
	tiles_x = tiles_x + 16
endwhile

// Get the input values

x = x + input.horizontal
y = y + input.vertical

//Check the character move direction

if input.horizontal > 0
    direction = 1
    animation_frame = animation_frame + 1
endif
if input.horizontal < 0
    direction = -1
    animation_frame = animation_frame + 1
endif

// animation_fame is a counter that is increases 1 per frame, so every 16 frames, change the character sprite to next walk sprite.

if animation_frame > 16
    animation = animation + 2
    if animation > 38
        animation = 32
    endif
    animation_frame = 0 
endif

// If the character doen't move, reset the sprite to idle

if input.horizontal == 0
    animation = idle
    animation_frame = 0 
endif

// px and py are a velocity modifier to character move

px = x / 4
py = y / 4

//Draw the character in screen

video.draw16 : px py animation direction

//Draw "HI!" in the scren with tiles

video.draw8 : 32 64 0 1
video.draw8 : 32 72 0 1
video.draw8 : 32 80 0 1
video.draw8 : 32 88 0 1
video.draw8 : 32 96 0 1
video.draw8 : 40 80 0 1
video.draw8 : 48 64 0 1
video.draw8 : 48 72 0 1
video.draw8 : 48 80 0 1
video.draw8 : 48 88 0 1
video.draw8 : 48 96 0 1

video.draw8 : 64 64 0 1
video.draw8 : 64 72 0 1
video.draw8 : 64 80 0 1
video.draw8 : 64 96 0 1

video.draw8 : 80 64 0 1
video.draw8 : 80 80 0 1
video.draw8 : 80 88 0 1
video.draw8 : 80 96 0 1


```


# Next Features

+ Emulator:
	+ Audio driver support
	+ ROM Generator
	+ Multiple spritesheets
	+ Mouse and joystick support
	+ Multiple layer on render for more performace
	+ Text writing on screen
+ Iti Machine
	+ Set to number greater or less than ±2.147.483.648 in builder
	+ Iti Machine Arduino vertion
+ KIS
	+ Import files keywords
	+ Comments support
	+ Expresstions in functions arguments





