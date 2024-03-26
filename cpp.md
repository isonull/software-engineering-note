# C/CPP

## CPP struct vs class

https://stackoverflow.com/questions/13049/whats-the-difference-between-struct-and-class-in-net#:~:text=Difference%20between%20Structs%20and%20Classes,are%20stored%20on%20the%20heap.&text=When%20you%20copy%20struct%20into,value%20of%20the%20other%20struct.
Structs are value type whereas Classes are reference type.
Structs are stored on the stack whereas Classes are stored on the heap. (NOT
TRUE, see https://stackoverflow.com/questions/3542083/reference-types-live-on-the-heap-value-types-live-on-the-stack)
Value types hold their value in memory where they are declared, but reference type holds a reference to an object memory.
Value types destroyed immediately after the scope is lost whereas reference type only the variable destroy after the scope is lost. The object is later destroyed by the garbage collector.
When you copy struct into another struct, a new copy of that struct gets created modified of one struct won't affect the value of the other struct.
When you copy a class into another class, it only copies the reference variable.
Both the reference variable point to the same object on the heap. Change to one variable will affect the other reference variable.
Structs can not have destructors, but classes can have destructors.
Structs can not have explicit parameterless constructors whereas classes can. Structs don't support inheritance, but classes do. Both support inheritance from an interface.
Structs are sealed type.

#define FOO(x) do { foo(x); bar(x); } while (0)

## this pointer

* retrieve hidden member variables
* return reference

## CString

allocate in the caller
allocate on the heap
static char * in the callee

varargs + type-info = overload

## CPP STL

### Container

list = double linked list
set = binary search tree

## C remainder %

property of %:
a == (a / b * b) + a % b

n general, GCC assumes that conditionals in if statements will be true - there
are exceptions, but they are contextual.

function call/return style:

Copy in and out to the position passed in by pointer
Return the copy of the data
Return the pointer of the data


Abbreviations in C:

ctor : constructor
dtor : destructor
itor : iterator
cset : copy set
cget : copy get
pget : pointer/position get

a translation unit (or more casually a compilation unit) is the ultimate input
to a C or C++ compiler from which an object file is generated

c js-closure for function ?

MIZAR project

C inline function in static library:

Use -O, see
https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html
define the inline function in .h
declare the extern of the inline function in .c
make sure the compiler support the semantics of such inline and extern, see
https://gcc.gnu.org/onlinedocs/gcc/Inline.html
https://stackoverflow.com/questions/17504316/what-happens-with-an-extern-inline-function
basically if the caller of the inline function is in the same compilation unit
as it and the optimisation of inline function is abled (gcc) then the inlined
version will be called if possible.
Otherwise, the caller will see the extern version in the object file and call
without optimisation. The inline optimisation is not possible when the caller
and the inline function is in difference compilation unit (caller do not see the
code!).

instruction cache (due to large instruction sizes) vs call cost

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


while 0 surrounded macro can be used in if else block while code block cannot

A function declaration is any form of line declaring a function and ending with
';'.(How to deal with the function symbol by its return type and name)
Eg.
int f(); declares the return type and name of a function but not the information
of arguments/inputs 
A prototype is a function declaration where all types of the parameters are
specified. (Basically the (arguments, return) type and name of the function)
Signature comprised of the name and parameter types (All information for calling
the subroutine)

Useful URLs:
https://blog.llvm.org/2011/05/what-every-c-programmer-should-know_14.html

https://en.wikipedia.org/wiki/Curiously_recurring_template_pattern