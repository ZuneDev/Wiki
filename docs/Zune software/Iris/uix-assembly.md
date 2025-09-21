# UIX Assembly
## Instruction Set
### Summary
#### General
Mneumonic | Name
--------- | ----
`COBJ` | [ConstructObject](#constructobject)
`COBJI` | [ConstructObjectIndirect](#constructobjectindirect)
`COBP` | [ConstructObjectParam](#constructobjectparam)
`CSTR` | [ConstructFromString](#constructfromstring)
`CBIN` | [ConstructFromBinary](#constructfrombinary)
`INIT` | [InitializeInstance](#initializeinstance)
`INITI` | [InitializeInstanceIndirect](#initializeinstanceindirect)
`LSYM` | [LookupSymbol](#lookupsymbol)
`WSYM` | [WriteSymbol](#writesymbol)
`WSYMP` | [WriteSymbolPeek](#writesymbolpeek)
`CSYM` | [ClearSymbol](#clearsymbol)
`PINI` | [PropertyInitialize](#propertyinitialize)
`PINII` | [PropertyInitializeIndirect](#propertyinitializeindirect)
`PLAD` | [PropertyListAdd](#propertylistadd)
`PDAD` | [PropertyDictionaryAdd](#propertydictionaryadd)
`PASS` | [PropertyAssign](#propertyassign)
`PASST` | [PropertyAssignStatic](#propertyassignstatic)
`PGET` | [PropertyGet](#propertyget)
`PGETP` | [PropertyGetPeek](#propertygetpeek)
`PGETT` | [PropertyGetStatic](#propertygetstatic)
`MINV` | [MethodInvoke](#methodinvoke)
`MINVP` | [MethodInvokePeek](#methodinvokepeek)
`MINVT` | [MethodInvokeStatic](#methodinvokestatic)
`MINVA` | [MethodInvokePushLastParam](#methodinvokepushlastparam)
`MINVAT` | [MethodInvokeStaticPushLastParam](#methodinvokestaticpushlastparam)
`VTC` | [VerifyTypeCast](#verifytypecast)
`CON` | [ConvertType](#converttype)
`OPR` | [Operation](#operation)
`ISC` | [IsCheck](#ischeck)
`ASC` | [As](#as)
`TYP` | [TypeOf](#typeof)
`PSHN` | [PushNull](#pushnull)
`PSHC` | [PushConstant](#pushconstant)
`PSHT` | [PushThis](#pushthis)
`DIS` | [DiscardValue](#discardvalue)
`RET` | [ReturnValue](#returnvalue)
`RETV` | [ReturnVoid](#returnvoid)
`JMPF` | [JumpIfFalse](#jumpiffalse)
`JMPFP` | [JumpIfFalsePeek](#jumpiffalsepeek)
`JMPTP` | [JumpIfTruePeek](#jumpiftruepeek)
`JMPD` | [JumpIfDictionaryContains](#jumpifdictionarycontains)
`JMPNP` | [JumpIfNullPeek](#jumpifnullpeek)
`JMP` | [Jump](#jump)
`CLIS` | [ConstructListenerStorage](#constructlistenerstorage)
`LIS` | [Listen](#listen)
`LISD` | [DestructiveListen](#destructivelisten)
`DBG` | [EnterDebugState](#enterdebugstate)

#### Math Operators
Mneumonic | Name
--------- | ----
`ADD` | [MathAdd](#mathadd)
`SUB` | [MathSubtract](#mathsubtract)
`MUL` | [MathMultiply](#mathmultiply)
`DIV` | [MathDivide](#mathdivide)
`MOD` | [MathModulus](#mathmodulus)
`NEG` | [MathNegate](#mathnegate)
`AND` | [LogicalAnd](#logicaland)
`ORR` | [LogicalOr](#logicalor)
`NOT` | [LogicalNot](#logicalnot)
`REQ` | [RelationalEquals](#relationalequals)
`RNE` | [RelationalNotEquals](#relationalnotequals)
`RLT` | [RelationalLessThan](#relationallessthan)
`RGT` | [RelationalGreaterThan](#relationalgreaterthan)
`RLE` | [RelationalLessThanEquals](#relationallessthanequals)
`RGE` | [RelationalGreaterThanEquals](#relationalgreaterthanequals)
`RIS` | [RelationalIs](#relationalis)
`INC` | [PostIncrement](#postincrement)
`DEC` | [PostDecrement](#postdecrement)

### Object instantiation
This family of instructions calls the constructor for the given object. All variants take the index of the object type to construct.

#### ConstructObject
Calls the default constructor with no parameters.
```asm
COBJ <typeIndex>
```

#### ConstructObjectIndirect
Used to create 'wrappers' of another constructed object. It is used for type schemas that implement `IDynamicConstructionSchema`, which appears to only be used by `UIClassTypeSchema` for creating the root `<UI>` element. The assignment type is taken from the first operand, while the target type is popped off the stack.
```asm
COBJI <assignmentTypeIndex>
```

#### ConstructObjectParam
Calls the specified constructor with *N* parameters popped from the stack, with the *N*th parameter at the top of the stack. The number of parameters must match the constructor exactly.
```asm
COBP <typeIndex>, <constructorIndex>
```

#### ConstructFromString
Uses the target's type converter to construct an instance from a constant string.
```asm
CSTR <typeIndex>, <stringIndex>
```

#### ConstructFromBinary
Uses the target type's binary decoder to construct an instance from a binary blob. The blob is stored inline and can be any length. The binary decoder is responsible for determining the length and consuming the entire blob.

#### InitializeInstance
Pops an object from the stack and initializes it using the specified type schema. This instruction is required to initialize `UIClass` and `Class` objects immediately after construction.
```asm
INIT <typeIndex>
```

#### InitializeInstanceIndirect
Identical to [InitializeInstance](#initializeinstance), except the type schema is popped off the stack rather than indexed from the type imports table.
```asm
INITI
```

### Symbols
This family of instructions manages symbols, usually local variables.

#### LookupSymbol
Looks up a symbol in the symbol reference table and pushes its value to the stack.
```asm
LSYM <symbolReferenceIndex>
```

#### WriteSymbol
Writes a value popped from the stack to the specified symbol in the symbol reference table.
```asm
WSYM <symbolReferenceIndex>
```

#### WriteSymbolPeek
Identical to [WriteSymbol](#writesymbol), except the value to be written is peeked instead of popped off the stack.
```asm
WSYMP <symbolReferenceIndex>
```

#### ClearSymbol
If the specified symbol is a scoped local, it is undeclared. Otherwise, this instruction is a no-op.
```asm
CSYM <symbolReferenceIndex>
```

### Properties

#### PropertyInitialize
Pops a value from the stack and initializes the specified property on the current instance, also popped from the stack.
```asm
PINI <propertyIndex>
```

#### PropertyInitializeIndirect
Simimlar to [PropertyInitialize](#propertyinitialize), except the parent type schema is popped from the stack.
```asm
PINII <propertyIndex>
```

#### PropertyListAdd
Pops a value from the stack and appends it to the end of a list. The key is read from the constants table. The target object is peeked from the stack. If `propertyIndex` is `0xFFFF`, the target object is the list itself. Otherwise, the list is read from the specified property on the target object.
```asm
PLAD <propertyIndex>
// target.Property.Add(value)

PLAD 0xFFFF
// target.Add(value)
```

#### PropertyDictionaryAdd
Pops a value from the stack and assigns it to the given key in a dictionary. The same semantics from [PropertyListAdd](#propertylistadd) regarding `propretyIndex` apply.
```asm
PDAD <propertyIndex>, <keyConstantIndex>
// target.Property[value] = value

PDAD 0xFFFF, <keyConstantIndex>
// target[key] = value
```

#### PropertyAssign
Assigns a value to the specified property. The target object is popped from the stack before the value is peeked.
```asm
PASS <propertyIndex>
; object.Property = value
```

#### PropertyAssignStatic
Peeks a value from the stack and assigns it to the specified static property.
```asm
PASST <propertyIndex>
; StaticInstance.Property = value
```

#### PropertyGet
Pops a parent object from the stack and pushes the value of the specified property.
```asm
PGET <propertyIndex>
; object.Property
```

#### PropertyGetPeek
Peeks an object from the stack and pushes the value of the specified property.
```asm
PGETP <propertyIndex>
; object.Property
```

#### PropertyGetStatic
Pushes the value of the specified static property to the stack.
```asm
PGETT <propertyIndex>
; StaticInstance.Property
```

### Methods

#### MethodInvoke
Invokes the specified method with *N* parameters popped from the stack, with the *N*th parameter at the top of the stack. The number of parameters must match the method signature exactly. The parent object is popped from the stack. If the method has a return value, it is pushed to the stack.
```asm
MINV <methodIndex>
```

#### MethodInvokePeek
Identical to [MethodInvoke](#methodinvoke), except the parent object is peeked instead of popped from the stack.
```asm
MINVP <methodIndex>
```

#### MethodInvokeStatic
Identical to [MethodInvoke](#methodinvoke), except the method must be static and no parent object is popped from the stack.
```asm
MINVT <methodIndex>
```

#### MethodInvokePushLastParam
Similar to [MethodInvoke](#methodinvoke), except the last parameter is pushed to the stack. When this mode is used, the method return value is not pushed to the stack.
```asm
MINVA <methodIndex>
```

#### MethodInvokeStaticPushLastParam
Similar to [MethodInvokeStatic](#methodinvokestatic), except the last parameter is pushed to the stack. When this mode is used, the method return value is not pushed to the stack. Equivalent to [MethodInvokePushLastParam](#methodinvokepushlastparam) but for static methods.
```asm
MINVAT <methodIndex>
```

### Type Manipulation
This family of instructions performs operations that manipulate types.

#### VerifyTypeCast
Throws a script runtime error if the object at the top of the stack cannot be cast to the specified type.
```asm
VTC <propertyIndex>
```

#### ConvertType
Pops an object from the stack and converts from one specified type to another. The converted object is pushed to the stack. Note that this is *not* equivalent to a simple cast: see [As](#as).
```asm
CON <toTypeIndex>, <fromTypeIndex>
```

#### IsCheck
Pops an object from the stack and checks whether it is an instance of the specified type. The check result is pushed as a `bool` to the stack. Returns `false` is the object is `null`.
```asm
ISC <typeIndex>
```

#### As
If the object at the top of the stack is assignable to the specified type, no action is taken. Otherwise, the object is replaced with `null`.
```asm
ASC <typeIndex>
```

#### TypeOf
Pushes the schema of the specified type to the stack.
```asm
TYP <typeIndex>
```

### Arithmetic and Logical Operations

#### Operation
Performs the specified arithmetic or logical operation using the provided operation host type schema. Unary operations will pop a single value off the stack and execute the operation. Binary operations will first pop the right operand, followed by the left. The operation result is pushed to the stack.

The operation host, such as `Int32Schema`, must support the requested operation. Type schemas can declare support for operations using `SupportsOperationHandler` and `PerformOperationHandler`.

This is a real instruction. It is recommended to use the alternative virtual instructions described in [Arithmetic and Logical Operations](#arithmetic-and-logical-operations) for the sake of clarity.
```asm
OPR <operationHostTypeIndex>, <operationId>
```

#### MathAdd
Pops two values off the stack and pushes their sum. This is a virtual instruction for [Operation](#operation).
```asm
ADD <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 1
```

#### MathSubtract
Pops two values, $a$ then $b$, off the stack and pushes their difference, $b - a$. This is a virtual instruction for [Operation](#operation).
```asm
SUB <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 2
```

#### MathMultiply
Pops two values off the stack and pushes their product. This is a virtual instruction for [Operation](#operation).
```asm
MUL <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 3
```

#### MathDivide
Pops two values, $a$ then $b$, off the stack and pushes their quotient, $b / a$. This is a virtual instruction for [Operation](#operation).
```asm
DIV <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 4
```

#### MathModulus
Pops two values, $a$ then $b$, off the stack and pushes their remainder, $b \bmod a$. This is a virtual instruction for [Operation](#operation).
```asm
MOD <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 5
```

#### MathNegate
Pops a value $a$ off the stack and pushes its negation, $-a$. This is a virtual instruction for [Operation](#operation).
```asm
NEG <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 16
```

#### LogicalAnd
Pops two values, $a$ then $b$, off the stack and pushes their logical AND, $a \land b$. This is a virtual instruction for [Operation](#operation).
```asm
AND <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 6
```

#### LogicalOr
Pops two values, $a$ then $b$, off the stack and pushes their logical OR, $b \lor a$. This is a virtual instruction for [Operation](#operation).
```asm
ORR <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 7
```

#### LogicalNot
Pops a value $a$ off the stack and pushes its logical negation, $\lnot a$. This is a virtual instruction for [Operation](#operation).
```asm
NOT <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 15
```

#### RelationalEquals
Pops two values, $a$ then $b$, off the stack and pushes whether they are equal, $b = a$. This is a virtual instruction for [Operation](#operation).
```asm
REQ <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 8
```

#### RelationalNotEquals
Pops two values, $a$ then $b$, off the stack and pushes whether they are not equal, $b \neq a$. This is a virtual instruction for [Operation](#operation).
```asm
RNE <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 9
```

#### RelationalLessThan
Pops two values, $a$ then $b$, off the stack and pushes whether the result of $b \lt a$. This is a virtual instruction for [Operation](#operation).
```asm
RLT <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 10
```

#### RelationalGreaterThan
Pops two values, $a$ then $b$, off the stack and pushes whether they are not equal, $b \gt a$. This is a virtual instruction for [Operation](#operation).
```asm
RGT <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 11
```

#### RelationalLessThanEquals
Pops two values, $a$ then $b$, off the stack and pushes whether they are less than or equal, $b \le a$. This is a virtual instruction for [Operation](#operation).
```asm
RLE <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 12
```

#### RelationalGreaterThanEquals
Pops two values, $a$ then $b$, off the stack and pushes whether they are greater than or equal, $b \ge a$. This is a virtual instruction for [Operation](#operation).
```asm
RGE <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 13
```

#### RelationalIs
Pops two values, `a` then `b`, off the stack and pushes whether `b is a`. This is a virtual instruction for [Operation](#operation), though it does not appear to be used anywhere.
```asm
RIS <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 14
```

#### PostIncrement
Pops a value $a$ off the stack and pushes $a + 1$. This is a virtual instruction for [Operation](#operation).
```asm
INC <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 17
```

#### PostDecrement
Pops a value $a$ off the stack and pushes $a - 1$. This is a virtual instruction for [Operation](#operation).
```asm
DEC <operationHostTypeIndex>
OPR <operationHostTypeIndex>, 18
```

## TODO

### `IStringEncodable`
> So I added an interface named `IStringEncodable` that has what's essentially a specialized `ToString()` method. Each schema has a matching runtime type, and if the schema supports converting from a string, I modify the runtime class to implement `IStringEncodable` and returns a valid string that UIX already knows how to parse.

> It's a pretty manual process, so I've been implementing that interface as I observe incorrect constants in the disassembly. Turns out the `Image` schema supports string encoding, but `UIImage` (the corresponding runtime class) didn't implement `IStringEncodable` so it has the wrong representation in the disassembly.
