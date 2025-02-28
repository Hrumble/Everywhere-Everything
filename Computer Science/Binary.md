*It's called binary because computer store values in `0` or `1`...so...2 values... binary?*

check [this](https://www.youtube.com/watch?v=PMpNhbMjDj0) to really get it.

***

A **Bit** is a value that is either `0` or `1`. They are used absolutely everywhere in the computer world, this is literally how everything works. 
>[!tip] Imagine an electronic circuit with a button that let's the current flow towards a LED or not.
>The light can either be `on` or `off` thus `1` or `0`. Press the button and the value is `1`, release it and the value is `0`. This is exactly what your CPU does except way faster than you.
>*and also without a lamp it just sends electricity or not to a certain device*

Everything can be represented with one or multiple **bits**, let's say for our case, a `0` means **no** and a `1` means **yes**, and let's play **smash or pass**.
Now our smash or pass is special, because we're a computer we can't actually see women, so we'll have to imagine them using `yes` or `no` questions:
- Does she have black hair?
- Does she have nails?
- Does she have male genitalia?
- Is she 5ft tall or more?
We can answer each of these questions using our `1` or `0` bits, so let's do it.
![[ana_armas.jpg|200]]
- Does she have black hair? `0` *not really*
- Does she have nails? `1` *hope so?*
- Does she have male genitalia? `0` ***HOPE SO??***
- Is she 5ft tall or more? `1` 

Great, we just described **Ana de Armas** with bits, according to us computer, ana is `0101`. 
*Pretty useless but basically that's what we use bits for*.

**Now in a more useful case, let's say using bits, we want to represent numbers up to $10$**
To do so we could use a series of 10 bits, each bit having a `1` or a `0` if the number is or isn't.
`0001000000` is 4!
**We did it! that's it!**
>[!warning] jk it fucking sucks it's terribly inefficient. have fun using $10^6$ bits to represent $10^6$.

we need a method that's more efficient, so let's say we keep the numbering system, 10 bits each representing numbers 1 through 10, but this time, let's sum each bit set to `1`.

| 0 | 0 | 1 | 1 | 0 | 0 | 0 | 1 | 0 | 0 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
so now our bits are `0011000100` and represent the value $3+4+8=15$  ! 
well that's way more efficient, we can represent numbers up to `1111111111` $\sum_{n=1}^{10} n=55$  !
*don't tell me you can't read that what are you 12?*
>[!tip] We can make it **EVEN MORE** efficient!

Firstly, let's say we want to represent the number $3$, we can either set the $2$ and $1$ **bit** to `1`, or set only the $3$ bits to `1`, knowing that, let's just remove the 3rd bit altogether because we can represent it with $2$ and $1$. 
We can't really get $4$ any other way so let's keep his bit, the numbers all the way through $7$ can all be substituted for a combination of $4, 2,$ and $1$ so let's remove all of them, and also 9 and 10. we're left with:

| 0 | 0 | 0 | 0 |
| ---- | ---- | ---- | ---- |
| 1 | 2 | 4 | 8 |
A cluster of 4**bits** (*it's called a Nibble btw*) able to represent numbers up to $15$! You can also see **each number is the last one multiplied by 2**, so the next in the sequence to represent higher numbers would be $8*2=16$
or more mathematically, the next power of two would be $2^4=16$

| 0 | 0 | 0 | 0 | 0 |
| ---- | ---- | ---- | ---- | ---- |
| 1 | 2 | 4 | 8 | 16 |
and we can now represent numbers up to $\sum_{n=0}^{4} 2^n= 31$. the more bits you add, the higher the number you can represent, and it **grows exponentially**, with 8 bits, you can represent numbers up to 255, and with 16 bits $\sum_{n=0}^{15} 2^n= 65535$ 
*PS: that's the max number of ports on a computer, because we use 16bits integers to reference them ;)*

>[!hot] let's go insane and do like the cool sha254 encryption, which uses 254 bits to encrypt stuff
>it can represent numbers up to $$\sum_{n=0}^{253} 2^n= 2^{254} -1 = 28948022309329048855892746252171976963317496166410141009864396001978282409983$$

*The part where $\sum_{n=0}^{253} 2^n= 2^{254} -1$ bothered me so much I couldn't believe it was true, turns out I've done some big boy 10th grade math and it does check out... still have no clue why tho but $\sum_{n=0}^{x} 2^n= 2^{x+1} -1$* so quick way to calculate max value I guess.

>[!info] Now that you know all that, understand that a **Byte** is simply an array of 8 bits 
>so max 255

## Big-Endian Little-Endian
>[!warning] Ok so far we've seen a **little-endian** system of byte reading, most computers actually work in **big-endian**
>>[!quote] Endianness refers to the order in which bytes are arranged to represent larger numerical values. There are two common types of endianness:
>>1. Big-endian: In big-endian systems, the most significant byte (the byte containing the highest-order bits) is stored at the lowest memory address. Subsequent bytes follow in decreasing order of significance.
>>2. Little-endian: In little-endian systems, the least significant byte (the byte containing the lowest-order bits) is stored at the lowest memory address. Subsequent bytes follow in increasing order of significance.
>>
>>In little-endian systems, when reading bytes individually, they are typically read from right to left. This is because the least significant byte is encountered first in memory.
>>In big-endian systems, bytes are read from left to right, starting with the most significant byte.
>>So, whether bytes are read from right to left or left to right depends on the endianness of the system.
>
>*this is from chatgpt thank you GPT please don't kill me when robots take over the world!*
>
>**All this is a fancy way of saying we read bytes from right to left** **IN MOST CASES**
>It's important to always check the endianness in the official spec of whatever you're trying to do, for instance, [[File Systems#FAT|FAT]] Uses little-endian, which means that data or any info on the disk is written in little endian binary.
>

let's have some fun byte reading using a **big-endian** system!

let's say the byte `00110101`

| 0 | 0 | 1 | 1 | 0 | 1 | 0 | 1 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
we got $$0*2^7 + 0*2^6 + 1*2^5 + 1*2^4 + 0*2^3 + 1*2^2 + 0*2^1 + 1*2^0 = 53$$
**yoo-hoo**?

***
# Read Binary Numbers

I know you're a lazy mf, the kind that doesn't like to waste time even though you spend half your days on video games, so let's see how to count up binary numbers fast, particularly **Bytes**.
*fuck 128bits who even uses that.*

First off, binary numbers are of course in **base 2**, which essentially means each number $n$ at the position $x$ is $n * 2^{x}$ and we write it $10101001_{2}$.
>[!info] a base is just a quick way of telling what number system we're dealing with and refers to how much possible values a digit can have
>Our common decimal system is in **base 10**, the number $124_{10}$ is read $4*10^0 + 2*10^1 + 1*10^2 =124$ 
>
>*Notice it's read from right to left just like bits!*
## Left to Right method
>[!quote] there is a nice left-to-right method for reading binary numbers: start at the left, and then each time you move rightward, you double your previous total and add the current digit. [...] I have found (and my students too), that with practice, this method is quicker than the right-to-left method.
>\- [a dude on math stack](https://math.stackexchange.com/questions/891445/why-binary-is-read-right-to-left) 

Let's say the byte `00101010`:
$0$
$0*2 + 0 = 0$
$0*2 + 1 = 1$
$1*2 + 0 = 2$
$2*2 + 1 = 5$
$5*2 + 0 = 10$
$10*2 + 1 = 21$
$21*2 + 0 = 42$

so `00101010` = $42$

## HEX
Hexadecimal is a base 16 numbering system. it's based on 4bit because it represents according to its own rules numbers from one to 15.
Numbers from 1 - 9 are written normally, as 1-9, but the double digits starting from 10 to 15 are **SPECIAL**

| A | 10 |
| ---- | ---- |
| B | 11 |
| C | 12 |
| D | 13 |
| E | 14 |
| F | 15 |
here's a hex number `0x3F` and it's equal to $15*16^0 + 3*16^1 = 63$
>[!info] the `0x` serves no purpose apart from telling programming languages we're about to give them a hex number and not just a stupid fucking string with no meaning

So just like normal bit groups in a [[#Big-Endian Little-Endian|Big-Endian]] system, Hex is read from right to left, multiplying each value with the following power of $16$, each letter represents a **Nibble**(4bits). so `0xF` is a **Nibble**, `0xFF` is a Byte, `0xFFFFFF` is 3 Bytes.
**most commonly used for colors, interpreted as 3 `0xFF` values rather than the entire `0xFFFFFF`each byte represents a value going from 0 - 255, intensity of Red Green and Blue**.
*either this or whoever gave you this number is toying with you because you'll have to calculate $\sum_{n=0}^{5} 15*16^n$ and I'm not doing it*

*it's 16843015 I got curious...*

# Definitions

**Bit** - A single digit either `1` or `0`
**Nibble** - A series of 4 **Bits**
**Byte** - A series of 8 **Bits** or 2 **Nibbles** (Non-formally defined)
**Octet** - Also a series of 8 **Bits**. (Formally defined)
**Word** - A series of 16 **Bits**
**DWord** - *Double Word* series of 32 **Bits**
**QWord** - *Quad Word* series of 64 **Bits**