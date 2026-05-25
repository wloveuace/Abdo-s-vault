
- Decimals are base 10 numbers  0->9
- Unsigned numbers are >= 0
- Signed numbers are <=0 or >=0 (Can represent negative and positive)
- Overflowing the size of a data type can lead to undefined behavior
- Floating point overflow will always be positive (Floats are wide and not precise unlike integers)
---
- byte is the smallest accessible storage unit in memory
- 1 byte range in integer is 0<255 and in hex its 00<`FF` (Hex is base 16 so from 0 till F(15))
- Even numbers in hex are easy to identify (single 1 value and rest is zeros ex: 8 in hex is 1000 , 2 is 0010 , 4 is 0100)
- 1 hex is represented by 4 bits
- Hex is counted from right side(most significant bit) to left size (least significant bit)
---
- Word size is the pointer size in a computer (x64 , word size is 8 bytes , Pointer size = 2^64) 
- Different data types can be used of different sizes 2-4-8 etc
- Pointer size is unsigned long - long represents the max size (8 bytes in x64)
- Data types sizes change based on the compiler settings (Arch) , fixed data sizes `intx_t` can be used to represent same data size no matter what compiler settings (`int64_t`) is 8 bytes on x64 and x86
- Float is single precision = 4 bytes | Double is double precision(standard data type used for fraction constants) = 8 bytes
---
- Multi-byte objects are stored as sequences of bytes in memory, often contiguously (including many multidimensional arrays).
- Multi-byte values can use two byte-order representations: `big-endian` and `little-endian`.
- Big-endian stores the most significant byte at the lowest memory address.
- Little-endian stores the least significant byte at the lowest memory address.
- Byte ordering should be considered when exchanging binary data between different computers or systems. (such as networking)
- Integers and floating-point numbers use different binary encoding schemes (e.g. two’s complement vs IEEE 754).
- Strings are encoded using character encodings such as ASCII or Unicode (e.g. UTF-8).

---
- Boolean algebra is defined over 2 sets of elements {0, 1} - encode logical values TRUE , FALSE
-  `~` - NOT , 0 becomes 1 and 1 becomes 0
- `&` - AND , if both values are 1 , result is 1
- `|` - OR , if one value is true , result is 1
- `^` - XOR , if only one value is true , result is 1
- Value XORED by itself is = 0
- ~0 will become bit vector or 1s (depending on word size)
- &FF - will create a mask with last 2 bits set
---
- Logical operators `&&` are different from bit-level operators `&`
- Logical operators will translate an expression to either a true or false value
---
- shift operators are either logical or arithmetic
- logical shifting to the left drops the most significant bits (on the left) and fills it with zero `(Xm-k) -> m is index , k is shift amount , X is index of the bit`
- logical shifting to the right drops the least significant bits (on the right) and fills it with zero `(X(m+k)/w) / 2 -> w is word size `
- arithmetic shifting to the right fills the most significant with the previous most significant bit ( 1010 >>> 2 = 1110)
---
- The C standards require the specification of integer data type (which has different sizes) and an optional indication of whether the variable will hold negative or positive value (it also increases the size)
- X is a bit vector -> $X = [X_{w-1} , X_{w-2},\dots,X_0]$
- Most significant bit has the value $2^{w-1}$ 
- Unsigned number encoding, where ${X_i}$ is the bit and each bit contributes by ${2^i}$ for a bit vector of width w  $$\Sigma^{w-1}_{i = 0}{x_i 2^i}$$
- in unsigned integers , every number between $0$ and $2^w-1$ is unique 
- Two complement encoding works by giving the most significant bit ($x_{w-1}$) a weight ($-2^{w-1}$) , if the bit is set then the number is negative

- Two complement encoding:$$(-2^{w-1})(x_{w-1}) + \Sigma_{i=0}^{w-2}x_i2^{i}$$
- if $(-2^{w-1})$ is set then it contributes with -8 and other bits being set decreases it
- UMAX - unsinged max = 2TMAX+1 - signed max = 255
- TMAX = -127 < TMAX < 127
- Most significant bit weigh is determined by the size so in 4 bytes (32 bits) MSB is $2^{31}$
- changing from signed to unsigned or the other way doesn't change any of the bits but the value represented changes , signed i = -1 , unsigned i = UMAX (same bit representation , different values)
- bit representation of UMAX  = TMIN (-1)
- conversion from two complement to unsigned $T2U(x) = x + 2^w$ - Negative -> (positive > TMAX), Positive -> positive
  ![[Pasted image 20260524054314.png]]

- conversion from unsigned to two complement $U2T(x) = x - 2^w$ , (Positive > TMAX) -> (Negative > TMIN), (positive  < TMAX) -> (positive < TMAX)
   ![[Pasted image 20260524054324.png]]

- depending on the appropriate directive (%d, %u , %x) used in printf , it uses the appropriate encoding scheme , `printf("%u", -1)`  = 4294967295 (UMAX = TMIN)
- when comparing unsigned and signed, signed variable becomes unsigned
- Converting an unsigned number from a small data type to a larger data type will always be positive , by adding leading zeros **zero extension**
- Converting a signed number from a small data type to a larger data type will fill the difference with the most significant bit $[x_{w-1},x_{w-1}\dots,x_{w-1},x_{w-2},x_{w-3}]$ , this is called **sign extension**
- sign extension leads to the expanding of the number while maintaining its value \[101] = -3 , \[1101] = -3 aswell , because as the number of bits increase the weight of the sign bit increase
- when changing from short to unsigned , the variable becomes an int not short 
- when truncating the size of a signed int from w to k, we drop the high order $w-k$ bits $(-\Delta w)$
- truncating removes MSBs no matter what, saturation removes bits but if 1 set bit is removed, all other bits are set
- when truncating unsigned bits we mod them with $2^k$ , where k is the bound of the bits so suppose k = 2 so any bit more than  $2^2$ is set to 0 $$\Sigma_{i=0}^{w-1}x_i2^{i}\bmod2^k = \Sigma_{i=0}^{k-1}x_i2^i$$ , K is the index of the new MSB bit
- ![[Pasted image 20260525050906.png]]

- when it comes to truncating signed integers, same concept applies but we need to change $2^{k-1}$ to $-2^{k-1}$ , give the MSB a negative weight
- truncating can lead to undefined behavior[^1]

---

[^1]: 
```c
#include <stdio.h>

#include <math.h>

  

int main(){

    int x = 2147483647; // 0111 1111 1111 1111 1111 1111 1111 1111

    x = (short)x; // 1111 1111 1111 1111 (65,535)

    printf("%hu", x); // 65,535

}
	```
