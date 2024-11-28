### Lox
>**standard expression**. can directly implement with recursive descent parser.
>- expression -> equality
>- equality -> comparison ( ("!=" | "== ") comparison)\*
>- comparison -> term ( (">" | ">=" | "<" | "<=") term )\*
>- term -> factor ( ( "-" | "+" ) factor ) \*
>- factor -> unary ( ("/" | "\*" ) unary ) \* 
>- unary -> ( "!" | "-" ) unary | call
>- call -> primary ( "(" arguments? ")" )*
>- arguments -> expression ( "," expression )*
>- primary -> NUMBER | STRING  | "true" | "false" | "nil" | "(" expression ")"
>**statement** -> exprStmt | printStmt | block
>- block -> "{" declaration* "}"
>- assignment     → ( call "." ) ? IDENTIFIER "=" assignment | logic_or ;
>**evaluate** evaluates the syntax tree. we do some type checking on tokens to catch errors. we create a class called **Interpreter** that implements the visit method for evaluation and execution. we have a code called **GenerateAst** that generates the **Expr** and **Stmt** abstract classes and specific instances like **Binary Expression** or **Variable**

```java
@Override
public Object visitAssignExpr(Expr.Assign expr){
	Object val=evaluate(expr.value);
	environment.assign(expr.name,value);
	return val
}
```


>**covariant arrays**:

compiles without error, but runtime check. 
```java
Object[] stuff = new Integer[1];
stuff[0]="not an int!"
```
>**variable lookup** - if token is not defined, we can make it a syntax error, runtime error, or allow it and return nil. static errors make it difficult to do recursion where functions call eachother.
>**l-value and r-value** - sometimes when we evaluate expressions we don't want to evaluate them.
>**scoping** - **environment** stores its parent environment. assignment occurs in current environment, and lookup happens in parent. interpreter stores current active environment and is updated when enters block.
>**implicit variable declaration** - creating variable and assigning to it
>- in python, assignment always creates new variable in current function's scope even if there's variable of same name outside
>**church-turning thesis** 
>**dangling else problem** solved by attaching else to nearest if
>**logic_and, logic_or** are binary operators that are implemented differently because of shortcircuiting, so they get their new **Expr.Logical** class
>**for loop** can be "**desugared**" into a while loop , no need to make a syntax node
>

>**earley parser**
>**shunting yard algorithm**
>**parsing expression grammar**
>**LL parser**
>**LR parser**
>**LALR parser**

>**returns** implemented as exceptions

```
Interpreter.visitReturnStmt()
Interpreter.visitIfStmt()
Interpreter.executeBlock()
Interpreter.visitBlockStmt()
Interpreter.visitWhileStmt()
Interpreter.executeBlock()
LoxFunction.call()
Interpreter.visitCallExpr()
```
we need to get from top of stack back to **call()** , we throw a **ReturnException**

>**closures** - we need to store the environment inside of a **LoxFunction** class that wraps the function statement and environment where function was declared
>**problem** - we can modify the closure environment of the function, so that future calls to the function work with an updated environment and not what the function was defined with.
>**solution** - create a new environment during every variable declaration
>**solution2** -  remember how many environments we need to walk to reach a variable. Implement a **Resolver** 
>- in interpreter, store a map from Variable Expressions (only local expressions) to depths (how many frames we need to go up our current frame to reach the correct frame where we access the variable) by walking through the whole program
>- prevent using a variable to define itself (local variable)
>- store the current function (FunctionType.None, FunctionType.Function) in Resolver to know what function if at all we are inside of. this can be used to check return statements
>- can implement checking for break only when inside a while loop similarly

edge case that is error:
```java
var a;
a=a;
```

>**class** - class declarations as a new syntax node. Create **LoxClass** which is what class name binds to. class creation would be like a function call, so we need **LoxClass** to implement **LoxCallable**. the function returns a **LoxInstance**
>- property access is now treated just like a function call in the syntactical grammar
>- **LoxInstance** has a map of its state, which we use for property access
>- **LoxClass** stores map from method name to **LoxFunction** methods

```java
class Egotist{
speak(){print this;}
}
var method=Egotist().speak;
method();
```

>we need to add **this** to the environment stored in **LoxFunction** for methods
>- this is resolved in resolver as a local variable. we put this inside our local scope when visiting a class.
>- when calling some method (**get**) on **LoxInstance**, we need to **bind** the current instance to "this" by creating a new environment containing the current closure with this defined to be the current instance
>- we forcibly make **init** return **this** by storing isInitialize in **LoxFunction**, and in **Resolver** we can force init to not return anything
>- static methods can be implemented by keeping another static method table and checking if an object is a class when visiting **visitGetExpr**

>**Superclass** is parsed as a **Expr.Variable** so that in **Resolver** we can resolve the superclass
>- Right before we define methods in Interpreter when visiting a class, we need to define "super" in its environment to map to the superclass (the super class gets recursively evaluated)
>- We need to **bind** the current instance to the superclass method as well when visiting **superExpr** in interpreter
>- we can statically check for **super** misuse by knowing if a class is a subclass
>- **traits** are a middle-ground btwn single and multiple inheritance, avoiding "diamond problem" where method resolution is ambiguous
>- Lox has subclass methods priority over superclass; to yup

### Clox
>Code representation as dynamic array of byte instructions **Chunk**. Value representation as dynamic array. 
>**stack-based** instruction set instead of register-based instruction set
>**VM Stack**
>- intermediate values during expressions
>- function arguments before calling function
>- return values after function finishes
>- local variables for all functions currently running
>**token**
>- type (TOKEN_ERROR or smth else)
>- start (where it starts in string)
>- length
>- line (which line)
>**keyword** identification using a trie (we can't use hash map like before)
>**advance(), errorAtCurrent(), error(), consume()** -> **emitByte(), emitBytes(), emitReturn(), endCompiler()**
>**pratt parser**
>- table given token type, gives function to compile a prefix expression starting with token of that type, function to compile infix expression whose left operand is followed by token of that type, precedence of infix expression using that token as operator
>- **parsePrecedence(precedence)** - try to parse prefix; try to parse infix. only care about things of higher precedence
>**Values**
>- VAL_NUMBER: type, padding, double
>- VAL_OBJ: type, padding, Obj* 
>**Objects** - strings need character array, instances need data fields, function needs bytecode chunk
>- the vm has a LinkedList storing every **Obj** which can be freed
>**Strings** can have complicated characters, extended grapheme cluster
>**Hashtables** for variable names used to force a length restriction on variable names
>- **FNV-1a** xors bytes??
>- deletion handled by a "tombstone" entry that enables linear probing to still work
>- **interning** solves the problem of duplicated strings wasting memory. stored in a hashtable in **vm.strings**
>**global variables** are different than **local variables** because they are late-bound. for **local variables**, we can bind the variable during compile time
>**global variables** are implemented by storing them in a hash table during compile time and runtime. Or using index based lookup during runtime and hash table during compile time.
>**statements** are like expressions that don't change num variables on stack
>- **expressions statements** are like expression + ";"
>- **print statements** evaluates an expression and prints the result
>**setters** need to look for the equal sign after the left expression, and need to check the left expression is a proper variable expression
>if we have **axb=c** the **variable()** function to parse a variable should not parse "=" when it parses **b**, which can be implemented by passing in **canAssign** to **every** parse function (because we're using a table of constant type)

>**local variables** - in design of lox, new locals only created by declaration statements, and declaration statements never inside expressions, so we can use stack for **locals** inside **Compiler** and indices to refer to them
>- **blocks** increase scopeDepth by 1
>- **creation/deletion of local variable in a block** is done at compile time. the local is the latest temporary on our runtime stack, so we just need to push the name and depth onto our locals stack
>- validation that local's name hasn't been defined before in the same scope
>- **declaring** is when a variable is added to the scope. its scopeDepth is set to a special sentinel -1
>- **defining** is when a variable can actually be used. the latest local's scope depth is updated.

```c
typedef struct{
Local locals[UINT8_COUNT];
int localCount;
int scopeDepth;
}Compiler* current;
typedef struct{
Token name;
int depth;
bool isCaptured;
}Local;
```

>**control flow** controls the **ip** field in the VM by emiting a **OP_JUMP_IF_FALSE** and **OP_JUMP** instructions to the byte stream followed by two bytes to store the offset. however, we don't know how much to jump ahead of time, so we compile the branch statement and then **patchJump**
>- **isFalsey** checks if the value (8 bytes on the stack, type-dependent interpretation) is false or not
>- we need to pop the condition expression, so we do that after **OP_JUMP_IF_FALSE** and **OP_POP**
>- implementing **and** by having left operand expression, **OP_JUMP_IF_FALSE**, **OP_POP**, right operand expression, ...
>- can implement **or** by using **OP_JUMP**, or just roundabout
>- can implement **while loop** using **OP_LOOP** and at end of loop since we're assuming unsigned jumps
>- **for loop** - first, begin a new scope. initializer can either be empty, **varDeclaration**, or **expressionStatement** . condition expression, **OP_JUMP_IF_FALSE**, etc.... for increment expression and body, a bit complex

>**function** has its own chunk that gets augmented when we're compiling the function
>to simplify, we can assume we're always writing to some function; this is stored in **Compiler**
>**CallFrame** with function, ip, slots created per call; ip stores the caller's own return address. stored on a stack with at most **FRAMES_MAX** items, like many languages there is max call depth
>When returning, the topmost **CallFrame** gets popped and the value stack's **numArgs** top elements get replaced by function's **result**. if there's no result the chunk stack gets **OP_NIL**
>**stack trace** by looping through the vm's frames and grabbing the current instruction in each frame and printing that out. Python puts innermost function last; others do opposite.
>**native functions** require new things because they don't push a **CallFrame**/has any bytecode chunk, so they get a new type of **ObjNative** with **Obj obj, NativeFn function**. they can be defined and inserted in **constant table**. can do validation that args are valid.
>ideally **ip** is stored in a register since it is being accessed thru the CallFrame pointer so often

>**closures** make variables that don't satisfy stack semantics since they get captured inside closures. a function is a property of a closure.
>**upvalues** stores locations of local variables that would otherwise be deleted. if we have three nested functions outer, middle, inner, and a local variable in outer, both middle and inner have upvalues capturing each other's upvalue
>- **resolveUpvalue** identifies and captures **upvalues** inside the enclosing compiler recursively. an **upvalue** **isLocal** if it is defined in teh enclosing compiler; otherwise, it is a grandparent or older
>- we **close upvalues** (convert them from pointing to value on stack to heap after stack gets popped) after we exit blocks through the **OP_CLOSE_UPVALUE** commands at end of block, and **OP_RETURN** after returning from a function  

```c
typedef struct{
Obj obj; int arity; Chunk chunk; ObjString* name;
} ObjFunction;
typedef struct{
ObjClosure* closure; uint8_t* ip; Value* slots;
}CallFrame;
```

>**closures** capture variables, not values. this code will print "updated" for both in most languages.
>**openUpValues** in vm is upvalue that points to a local variable still on the stack 
>**closed upvalue** is when variable moves to heap
>**ObjUpvalue** - variables that aren't used by closures live and die on the stack
>**sharing** of upvalues is implemented by a linked list of **ObjUpValue**, stored inside itself, so that if we're trying to capture some **local** value, we check if we already have an upvalue for that

```c
var globalSet;
var globalGet;
fun main() {
  var a = "initial";
  fun set() { a = "updated"; }
  fun get() { print a; }
  globalSet = set;
  globalGet = get;
}
main();
globalSet();
globalGet();

```

>**roots** are any object that a VM can reach directly without going thru a reference in some other object. any object referred to from reachable object is reachable.
>**sophisticated garbage collectors** are run on separate thread or interleaved periodically during execution, often at function call boundaries or when backward jump occurs
>we mark the global variable hashtable both keys and values, open upvalue list, closures in the CallFrame stack
>**subtle memory bug** - in addConstant, when calling `writeValueArray(&chunk->constants, value)`, should push value onto stack first (and then undo) because this enables this value to be garbage collected should the constants array get expanded and garbage collection triggered. same with **string interning** and **concatenating strings** where we need to keep these values on stack or else they'll get sweeped away. 
>**generational garbage collectors** separate memory into two groups, short-lived and long-lived

>**OOP**
>- **ObjClass** added to the VM, can create class with some name
>- **robust to following cases** like `var obj = "not instance"; print obj.field`. uses **OP_GET_PROPERTY** and **OP_SET_PROPERTY** and reads from frame chunk to assign variable properly
>- **Inline cache** - cache the index in the method hashtable to a method instead of having to do lookup by name in hashtable every time
>- when adding methods, we need to create a closure with **this** defined, and also keep track to the object calling the method. hence, **OBJ_BOUND_METHOD** struct with **Obj ob, Value receiver, ObjClosure* method**
>- on the stack, we pass the **receiver** of bound method as the 0th argument which is kept free
>- **forbid this** outside of a class by storing linked list of current enclosing class in our compiler
>- **initializer** methods have a different type than normal methods, since they must return the newly constructed object rather than the default return value, so they emit an **OP_GET_LOCAL** to the instructions
>most of the time, we can **fuse** **OP_GET_PROPERTY** and **OP_CALL** into **OP_INVOKE**
>- **callValue(value, argCount)** calls value, hopefully a closure, if it is found in the instance field table
>- **invokeFromClass(instance->klass, name, argCount)** calls the instance method by looking it up in the class table
>- **optimize init** by storing directly inside the class object instead of having to do table lookup like other methods
>**inheritance**
>- clox does **copy-down** inheritance. since classes can't get modified after declaration, when doing inheritance, we can copy the methods from superclass into subclass. this is before compiling subclass methods, so overriding will happen.
>- if **fields** collide, **namespace lookup** will **compile-time field name check** will check no two classes have the same name, **namespaces for fields** will prepend classnames to field, avoiding collisions

>**optimize** interning by storing small enough strings inline in the value rather than on heap
>- use bit tricks instead of modulo in the hashtable
>- **Nan boxing** uses that 8-byte doubles have 51 extra bits not storing any information, and we only need 48 bits for memory
### Architecture
>**cache line** - **tag, index, block offset**. the index indexes into the set; the set stores **ways** elements
>**__builtin_prefetch(addr)** pre-fetches cache line at addr. also the L1 or L2 prefetchers will perform prefetching automatically, and typically 10 L1 LFBs (can prefetch 10 cache lines at a time) and 24 L2 LFBs. Latency of L1, L2, L3, memory is 1.6ns (3 cycles), 5.6ns, 28ns, 120ns, and capacity of L1, L2, L3 cache is 32KB, 1MB, 32MB
>**funnel-sort** sorts $n^{1/3}$ groups of $n^{2/3}$ items and merges. Cache Complexity is $n/B\log_{M}n$ where $B$ is cache line size in elements and $M$ is cache size in elements, which is asymptotically optimal and improves upon naive mergesort by $\log M$ 
>**CAS(*result, old, new)*** compares old to \*result and sets \*result to new if equal
>- **lock-free** push and pop with CAS
>- optimize further by testing if \*result is old and then doing the CAS, and exponential backoff
>**MESI** protocol
>- **Modified** - cache line is only in current cache and dirty
>- **Exclusive** - cache line is only in current cache, but is clean
>- **Shared** - may be stored in other caches and is clean
>- **Invalid** 
>**ABA** problem in multi-threaded computing
>- in a lock-free DS, when item is removed, deleted, and new item is added, it is common for new item to be at same location, causing problems
