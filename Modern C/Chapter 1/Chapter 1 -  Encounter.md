
## What's programming in C

Programming in C is about having the computer complete some specific tasks. A C program does that by giving orders, much as we would express such orders in the imperative tense in many human languages, thus the term **imperative programming** for this particular way of organizing computer programs.

Imperative programming is a design that's made to link between computers instructions and high level English and human language 

C consists of **statements** or instructions which can be calling functions , performing math ,  logic, ETC., **literals** which are formats for output , **Constants** which are names that are translated to values , **expressions** which are assigning variables , creating them , comparing them and more

C provides an abstraction to be ran on different operating systems with different binary formats , that's because its a compiled language (some cases may apply) , in general a C program that doesn't require a certain OS should run on any OS

---

## The structure of a C program

This section discusses the C grammar, i personally interpret grammar as the language's syntax + logic + rules

**Comments** (\/\*comment\*\/  , //Comment ) Comments are ignored by the compiler, so it is the perfect place to explain and document your code. Such in-place documentation can (and should) greatly improve the readability and comprehensibility of your code

**Literals** Our program contains several items that refer to fixed values that are part of the program: 0, 1, 3, 4, 5, 9.0, 2.9, 3.E+25, .00007, and "element %zu is %g, \tits square is %g\n". These are called literals.

**Identifiers** they are basically any name (variable name , function name , definition name (also called constants), data type name , Enum members (also called constants), ETC. )

**Functions** they are blocks of code that can be called by just a name , they have **Declaration** which defines the overhead of the function `int foo(char x)` , followed by a **Definition** which is the actual code the function executes when called, Declaration follows this form `ret-type name(parameters)`

**Operators** C has multiple operators , such as initialization & assignment operator '=' , comparison operators ' > , < ' ETC.

**Attributes** Attributes such as `[[maybe_unused]]` are placed into double square brackets as shown and provide some supplemental information to the principle structure of the program (this is a c23 feature)

**Now we have known the C fundamental syntax blocks , we will dive into more definitions**

---

==**Declarations**== 

Before we may use a particular identifier in a program, we have to give the compiler a declaration C that specifies what that identifier is supposed to represent (Data type), usually
identifier + data type is called a declaration `int x` , that's a declaration variable called x that will hold an integer value, it also instructs the compiler to save some amount of storage for that variable in memory

![[Pasted image 20260304060806.png]]

Conceptually, it is important to distinguish the box itself (the object), the specification (its type), the box contents (its value), and the name or label that is written on the box (the identifier).

**Declarations scope**
Declarations are also bounded to the scope which is defined by `{}` where '{' is the start and '}' is the end of the scope , same initializer cant have multiple declarations within one scope , initializer declarations reset from scopes , nested scopes are an exception which will be explained below

Functions must have a scope where we write the **Code block** eventually we can declare variables in the function , but we also need to consider something like loops, which is used in programming logic , they have their own scope inside the main function scope - in this care applies the nested scope rules where inner scope (**primary block**) can access outsider scope (**Secondary block**) declarations but not other wise also when the inner loop goes out of scope the declarations made inside it are reset (removed)

**Global declarations** they are declarations that don't reside inside a scope , they can be accessed in any scope within the code , and they cant have multiple declarations as well (they sit in the **file scope**)

---

==**Initialization**==

After declaring variables we should assign them some values , this process in called *initialization*
it should be straight forward and easy to understand. Once we declare a variable `int x` we then use the assignment operator '=' which in return assigns the value after is to the variable declared
`int x = 10;` now x is equal to 10 , when we don't initialize values (the default value initialized is 0)

in arrays we can do initialization to multiple variables at once for example
```c
int Arr[4] = {
 [0] = 1,
 [1] = 2,
 [2] = 3,
};
```
This is called **designated initialization** , arr\[3] is initialized to 0 automatically

---

==**Satatmenets**==

Statements. The second part of the main function consists primarily of statements. Statements are instructions that tell the compiler what to do with identifiers that have been declared so far

loops such as the for loop are also considered statements This is the simplest form of domain iteration that C has to offer. For loops consists of 3 expressions 
`for(declaration + init(expression), loop condition(expression), statment)` so 2 expressions and a statement ( they can be more but thats the base of it )

---
==**Functions**==

Functions are the base of any programming language , in C they are straight forward, we discussed the function header and primary scope of a function before, we will just give a template of what a functions looks like once more

```c
int foo(char x){
 x++
 return x
}
```

here this function has a return type of int , take an argument of a character and gives it the name x then in the function body it does 2 statements, 1st it increases x (as chars interpreted as ints internally ) then it returns the modified x value which is an int 

---
## Footer

in my opinion, lame chapter , skip it guys 😂